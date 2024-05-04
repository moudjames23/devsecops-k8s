  pipeline
  {
        agent any

        stages {
            stage('Build Artifact')
            {
                steps
                {
                  sh "mvn clean package -DskipTests=true"
                  archive 'target/*.jar' //so that they can be downloaded later
                }
            }

            stage('Unit Tests -  JUnit and Jacoco')
            {
                steps
                {
                  sh "mvn test"
                  archive 'target/*.jar' //so that they can be downloaded later
                }

                post
                {
                    always
                    {
                        junit 'target/surefire-reports/*.xml'
                        jacoco execPattern: 'target/jacoco.exec'
                    }
                }

            }

            stage('Build and Push Docker Image')
              {
                  steps
                  {
                    withDockerRegistry(['credentialsId': 'dockerhub', 'url': ''])
                    {
                      sh 'docker build -t moudjames23/devsecops-k8:""$GIT_COMMIT"" . ' 
                      sh 'docker push moudjames23/devsecops-k8:""$GIT_COMMIT""'
                    }
                    
                  }
              }

              stage('K8s deployment - dev')
              {
                  steps
                  {
                    withKubeConfig(['credentialsId': 'kubeconfig'])
                    {
                      sh "sed -i 's#replace#moudjames23//devsecops-k8:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
                      sh 'kubectl apply -f k8s_deployment_service.yaml'
                    }
                    
                  }
              }
        }
  }

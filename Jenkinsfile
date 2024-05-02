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
                        jacoco exexPattern: 'targer/jacoco.exec'
                    }
                }

            }
        }
  }

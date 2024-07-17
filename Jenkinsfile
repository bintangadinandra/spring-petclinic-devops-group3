pipeline {
    agent any 

    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!' 
            }
        }

        stage("Sonarqube Analysis"){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Petclinic \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Petclinic '''
                }
            }
        }

        stage('SonarQube Analysis Automate'){
            environment{
                SNQ_TOKEN = readFile('/var/jenkins_home/jobs/.sonar-token')
                SNQ_IP = "${env.SNQ_IP}"
                PROJECT_NAME = "${env.PROJECT_NAME}"
            }
            steps{
                sh './mvnw sonar:sonar -Dsonar.projectKey=${PROJECT_NAME} -Dsonar.host.url=http://${SNQ_IP}:9000 -Dsonar.login=${SNQ_TOKEN}'
            }
        }
    }
}

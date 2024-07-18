pipeline {
    agent any 

    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!' 
            }
        }

        // stage("Sonarqube Analysis"){
        //     environment {
        //         SCANNER_HOME=tool 'sonar-scanner'
        //     }
        //     steps{
        //         withSonarQubeEnv('sonar-server') {
        //             sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Petclinic \
        //             -Dsonar.java.binaries=. \
        //             -Dsonar.projectKey=Petclinic '''
        //         }
        //     }
        // }

        stage('SonarQube Analysis Automate'){
            environment{
                SNQ_TOKEN = readFile('/var/jenkins_home/jobs/.sonar-token')
                SNQ_IP = "sonarqube"
                PROJECT_NAME = "Spring-Petclinic"
            }
            steps{
                sh './mvnw clean package sonar:sonar -Dsonar.projectKey=${PROJECT_NAME} -Dsonar.host.url=http://${SNQ_IP}:9000 -Dsonar.login=${SNQ_TOKEN}'
            }
        }
    }
}

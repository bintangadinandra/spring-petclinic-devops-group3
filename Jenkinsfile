pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        // stage('SonarQube Analysis') {
        //     environment {
        //         SNQ_IP = "sonarqube"
        //         PROJECT_NAME = "Spring-Petclinic"
        //     }
        //     steps {
        //         withCredentials([string(credentialsId: 'Sonar-token', variable: 'SNQ_TOKEN')]) {
        //             sh './mvnw clean package sonar:sonar -Dsonar.projectKey=${PROJECT_NAME} -Dsonar.host.url=http://${SNQ_IP}:9000 -Dsonar.login=${SNQ_TOKEN}'
        //         }
        //         echo "(Please use this) Host SonarQube Dashboard URL: http://localhost:9000/dashboard?id=${env.PROJECT_NAME}"
        //     }
        // }

        stage('Deploy to EC2') {
            steps {
                ansiblePlaybook(
                    playbook: 'deploy.yml',
                    inventory: 'inventory',
                    credentialsId: '3b2c0f43-795b-42dd-9b38-372e90573878'
                )
            }
        }
    }
}

pipeline {
    agent any 

    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!' 
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SNQ_IP = "sonarqube"
                PROJECT_NAME = "Spring-Petclinic"
            }
            steps {
                withCredentials([string(credentialsId: 'Sonar-token', variable: 'SNQ_TOKEN')]) {
                    sh './mvnw clean package sonar:sonar -Dsonar.projectKey=${PROJECT_NAME} -Dsonar.host.url=http://${SNQ_IP}:9000 -Dsonar.login=${SNQ_TOKEN}'
                }
                echo "(Please use this) Host SonarQube Dashboard URL: http://localhost:9000/dashboard?id=${env.PROJECT_NAME}"
            }
        }

        stage('Build Project') {
            steps {
                sh './mvnw clean package'
            }
        }
        stage('Deploy') {
            steps {
                ansiblePlaybook credentialsId: 'ansible-vault', playbook: 'deploy.yml', inventory: 'inventory/production'
            }
        }
    }
}

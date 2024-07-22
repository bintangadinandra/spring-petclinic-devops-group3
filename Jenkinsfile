pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!' 
            }
        }
        stage('Run OWASP ZAP') {
            steps {
                script {
                    // Pull the OWASP ZAP Docker image
                    sh 'docker pull zaproxy/zap-bare'

                    // Run OWASP ZAP Docker container
                    sh 'docker run --platform linux/arm64 --name zap -p 8090:8090 -i zaproxy/zap-bare zap.sh -cmd -port 8090 -quickurl http://www.google.com -quickout /zap/zap-report.html'

                    // Give OWASP ZAP some time to start
                    sleep 30

                    sh 'docker cp zap:/zap/zap-report.html ${WORKSPACE}/zap-report.html'

                    // Stop and remove the OWASP ZAP container
                    sh 'docker stop owasp-zap'
                    sh 'docker rm owasp-zap'
                }
            }
        }
    }
}

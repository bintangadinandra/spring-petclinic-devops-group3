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
                    sh 'docker run -u zap -d -v $(pwd):/zap/wrk/:rw --name owasp-zap zaproxy/zap-bare zap.sh -daemon -host 0.0.0.0 -port 8080 -config api.disablekey=true'

                    // Give OWASP ZAP some time to start
                    sleep 30

                    // Run your security tests here, e.g., using ZAP CLI or API
                    // Example: sh 'docker exec owasp-zap zap-cli quick-scan http://yourapp'
                    sh 'docker exec owasp-zap zap-cli quick-scan --self-contained --start-options "-config api.disablekey=true" http://yourapp'

                    // Stop and remove the OWASP ZAP container
                    sh 'docker stop owasp-zap'
                    sh 'docker rm owasp-zap'
                }
            }
        }
    }
}

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
                        try {
                            sh './mvnw package -DskipTests'
        
                            sh 'java -jar target/*.jar --server.port=8085 & echo $! > java_pid.txt &'
        
                            sleep 30
                            
                            // Pull the OWASP ZAP Docker image
                            sh 'docker pull zaproxy/zap-stable'

                            try {
                            // Run OWASP ZAP Docker container
                            sh 'docker run -t --name owasp-zap zaproxy/zap-stable zap-baseline.py -t http://$(hostname -i):8085 -r results.html'
                            } catch (Exception e) {
                                echo 'Ignore error during ZAP script.'
                                sh 'pwd'
                                sh 'ls'
                            }

                            sh 'docker cp owasp-zap:/zap/results.html ${WORKSPACE}/zap-report.html'
                            
                        } finally {
                            
                            // Kill the Java process
                            sh 'kill $(cat java_pid.txt) || true'

                            // Clean up containers
                            sh 'docker stop owasp-zap || true'
                            sh 'docker rm owasp-zap || true'
                        }
                }
            }
        }
    }
}

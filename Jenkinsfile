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
                    sh './mvnw package -DskipTests'

                    sh 'java -jar target/*.jar --server.port=8085 & echo $! > java_pid.txt &'

                    sleep 30
                    
                    // Pull the OWASP ZAP Docker image
                    sh 'docker pull zaproxy/zap-stable'

                    // Run OWASP ZAP Docker container
                    sh "docker run -t zaproxy/zap-stable zap-baseline.py -t http://$(ip -f inet -o addr show docker0 | awk '{print $4}' | cut -d '/' -f 1):8085"

                    sh 'docker cp zap:/zap/zap-report.html ${WORKSPACE}/zap-report.html'

                    // Kill the Java process
                    sh 'kill $(cat java_pid.txt)'
                }
            }
        }
    }
}

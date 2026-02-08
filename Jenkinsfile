pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    def app = docker.build("html-app:${env.BUILD_ID}")
                    // Tag & push (optional)
                    // docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
                    //     app.push("latest")
                    // }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Stop old, run new
                    sh '''
                        docker stop html-app || true
                        docker rm html-app || true
                        docker run -d -p 9090:80 --name html-app html-app:${BUILD_ID}
                    '''
                }
            }
        }
        stage('Test') {
            steps {
                sh 'curl -f http://localhost:9090 || exit 1'
            }
        }
    }
    post {
        always {
            sh 'docker logs html-app || true'
        }
    }
}

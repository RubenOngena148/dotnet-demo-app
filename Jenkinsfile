pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                sh 'docker compose down || true'
            }
        }
        stage('Build and deploy') {
            steps {
                sh 'docker compose up -d --build'
            }
        }
        stage('Test') {
            steps {
                sh '''
                for i in $(seq 1 30); do
                    if curl -fs http://172.16.0.10:8081/ > /dev/null; then
                        echo "App is bereikbaar"
                        exit 0
                    fi
                    sleep 2
                done
                echo "App niet bereikbaar"
                docker compose logs
                exit 1
                '''
            }
        }
    }
}
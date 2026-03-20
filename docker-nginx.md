---
            
pipeline {
    agent any
    
    stages {
        stage('pull nginx image') {
            steps {
                sh 'docker pull nginx:latest'
            }
        }
        
        stage('create hmtl file') {
            steps {
                sh '''
                mkdir -p html
                cat <<EOF > html/index.html
                 <h1>Hello World from Jenkins 🚀</h1>
                EOF
                
                 '''
            }
        }

        stage ('run nginx container') {
            steps {
                sh '''
                docker rm -f nginx:latest || true

                docker run -d --name nginx1 -p 8090:80 -v $(pwd)/html:/usr/share/nginx/html nginx:latest

                '''
            }
        }

        stage('test web page') {
            steps {
                sh '''
                sleep 5
                curl http://localhost:8090
                '''
            }
        }
    }
}         


---

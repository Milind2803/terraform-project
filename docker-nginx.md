Install docker
```
sudo apt install docker.io -y
```
```
sudo systemctl start docker
sudo systemctl enable docker
```
```
sudo usermod -aG docker jenkins
```
```
sudo systemctl restart docker
sudo systemctl restart jenkins
```
```
sudo su - jenkins
docker ps
```



```
            
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
```

```
http://<your-server-ip>:8080
```

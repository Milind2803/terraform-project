```
pipeline {
    agent any

    stages {

        stage('Pull Image') {
            steps {
                sh 'docker pull mukunddeo9325/super-mario'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker run -d \
                      --name my-app \
                      -p 8081:80 \
                      mukunddeo9325/super-mario
                '''
            }
        }
    }
}
```

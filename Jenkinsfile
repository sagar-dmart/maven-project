pipeline {
    agent any
    stages{
        stage('Build') {
            steps {
                sh 'mvn clean package'
                sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
                sh "docker run --detach -it -p 8082:8080 tomcatwebapp:${env.BUILD_ID}"
            }
        }
    }
}
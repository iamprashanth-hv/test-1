pipeline {
    agent any
    tools{
        jdk 'java-11'
        maven 'maven'
    }
    stages{
        stage('git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/iamprashanth-hv/test-1.git'
            }
        }
        stage('compile'){
            steps{
                sh "mvn compile"
            }
        }
        stage('Build'){
            steps{
                sh "mvn clean install" 
            }
        }
        stage('Build and Tag Docker file'){
            steps{
                sh "docker build -t iamprashanth05/project:1 ."
            }
        }
        stage('Docker image scan'){
            steps{
                 sh "trivy image --format table -o trivy-image-report.html iamprashanth05/project:1"
            }
        }

        stage('Containersation'){
            steps{
                sh '''
                    
                    docker run -it -d -p 9009:8080 iamprashanth05/project:1
                '''
            }
        }

        stage('Login to Docker Hub') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                            }
                        }
                    }
        }
        stage('Pushing image to repository'){
            steps{
                sh 'docker push iamprashanth05/project:1'
            }
        }
    }
}

pipeline{
    agent any
    stages{
        stage('git Clone'){
            steps{
                git 'https://github.com/haroon0403/CI-CD-jenkins-docker.git'
            }
        }
        stage('Execute mvn'){
            steps{
               sh 'mvn package'
            }
        }
        stage('Docker Build and Tag'){
            steps{
                sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID 0403456/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID 0403456/$JOB_NAME:latest'
                sh 'docker rmi 0403456/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker rmi $JOB_NAME:v1.$BUILD_ID '
            }
        }
     /*   stage('push image to docker hub'){
            steps{
                withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhubpassword')]) {
                sh 'docker login -u 0403456 -p $dockerhubpassword'
              //sh 'docker image push 0403456/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image push 0403456/$JOB_NAME:latest'
                
             } 
        }  */
        stage('Docker stop and remove'){
            steps{
                sh 'docker ps -q -f status=running | xargs --no-run-if-empty docker container stop'
                sh 'docker ps -q -f status=exited | xargs --no-run-if-empty docker rm'
            }
        }
        stage('Run Docker container on Jenkins Agent'){
            steps{
              //sh "docker run -d -p 8003:8080 0403456/$JOB_NAME:v1.$BUILD_ID"
                sh "docker run -d -p 8003:8080 0403456/$JOB_NAME:latest"
            }
         }
         
        stage('Deploy on kubernetes through ansible'){
            steps{
                sh "ansible-playbook /ansible/task.yaml"
            }
        }

        }

    }

pipeline {
    agent any
    stages {
        stage('git_clone'){
            steps{
                git branch: 'master', url: 'https://github.com/vsk2408/maven-web-app.git'
            }
        }
        stage('build'){
           steps{
               sh 'mvn clean install -X -U'
           }
       }
       stage('docker_build'){
          steps{
              sh 'docker build -t maven-web-app -f Dockerfile .'
              //sh 'docker save -o spring-boot-app.tar spring-boot-app:latest'
              //sh 'chown jenkins:jenkins spring-boot-app.tar'
          }
       }
       stage('Pushing Image') {
            steps {
                script {
                    sh '''aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 723185050974.dkr.ecr.ap-south-1.amazonaws.com
                    docker tag maven-web-app:latest 723185050974.dkr.ecr.ap-south-1.amazonaws.com/sping-pet-clinic-repo:latest
                    docker push 723185050974.dkr.ecr.ap-south-1.amazonaws.com/sping-pet-clinic-repo:latest'''
                }
            }
       }
        stage('Deployment') {
            steps {
                sshagent(['Kube_Master']) {
                    sh 'scp -o StrictHostKeyChecking=no maven-web-app-deploy.yml ubuntu@172.31.3.42:/home/ubuntu/'
                    script {
                        try{
                            sh 'ssh ubuntu@172.31.3.42 kubectl apply -f .'
                        } catch(error){
                            sh 'ssh ubuntu@172.31.3.42 kubectl create -f .'
                        }
                    }
                }
            }
        }
    }
}    

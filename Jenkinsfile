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
    }
}    

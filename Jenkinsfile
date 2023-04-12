pipeline{
    agent any
    tools {
        maven 'Jenkins_Maven'
    }
    stages {
        stage('Build Maven') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'pruthv1k', url: 'https://github.com/pruthv1k/jenkins.git']])

                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t devopshint/my-app-1.0 .'
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u pruthv1k -p ${dockerhubpwd}'
                 }  
                 sh 'docker push devopshint/my-app-1.0'
                }
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout GitHub repo') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Heropon33/tp-devops']])
            }
        }
        
        stage('Build and Tag Docker Image') {
            steps {
                script {
                    sh 'docker build . -t hellodocker'
                    sh 'docker tag hellodocker heropon33/hellodocker'
                }
            }
        }
        
        stage('Push the Docker Image to DockerHUb') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_token', variable: 'docker_token')]) {
                    sh 'docker login -u heropon33 -p ${docker_token}'
}
                    sh 'docker push heropon33/hellodocker'
                }
            }
        }
        
        stage('Deploy deployment and service file') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f deploymentsvc.yaml'
                    }
                    #kubernetesDeploy configs: 'deploymentsvc.yaml', kubeconfigId: 'kubeconfig'
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        IMAGE_NAME = "heropon33/hellodocker"
    }

    stages {
        stage('Checkout GitHub repo') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], 
                userRemoteConfigs: [[url: 'https://github.com/Heropon33/tp-devops']])
            }
        }

        stage('Build and Tag Docker Image') {
            steps {
                script {
                    env.IMAGE_TAG = "${IMAGE_NAME}:v${env.BUILD_NUMBER}"
                    sh "docker build . -t ${env.IMAGE_TAG}"
                }
            }
        }

        stage('Push the Docker Image to DockerHUb') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_token', variable: 'DOCKER_TOKEN')]) {
                        sh """
                            docker login -u heropon33 -p ${DOCKER_TOKEN}
                            docker push ${env.IMAGE_TAG}
                        """
                    }
                }
            }
        }

        stage('Deploy deployment and service file') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_FILE')]) {
                        sh """
                            mkdir -p ~/.kube
                            cp \$KUBECONFIG_FILE ~/.kube/config
                            kubectl get nodes
                            kubectl get deployment hellodocker-deployment || kubectl apply -f deploymentsvc.yaml
                            kubectl set image deployment/hellodocker-deployment hellodocker=${env.IMAGE_TAG} --record
                            kubectl rollout status deployment/hellodocker-deployment
                        """
                    }
                }
            }
        }
    }
}


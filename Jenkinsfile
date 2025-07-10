pipeline {
  agent {
    docker { 
      image 'php:8.4.8-alpine3.22' 
    } 
  }
  stages {
    stage('construction') {
      steps {
        sh 'php --version'
      }
    }
  }
}

pipeline {
  agent {
    node {
      label 'jkagent'
    }
  }
  stages {
    stage('construction') {
      agent {
        docker { 
          image 'php:8.4.8-alpine3.22' 
        } 
      }
      steps {
        sh 'php --version'
      }
    }
  }
}

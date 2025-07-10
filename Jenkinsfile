pipeline {
  agent {
    node {
      label 'jkagent'
    }
  }
  stages {
    stage('construction') {
      steps {
        sh 'docker version'
      }
    }
  }
}

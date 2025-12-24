pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        bat 'npm run build'
      }
    }
    stage('Test') {
      steps {
        bat 'npm run test'
      }
    }
  }
}

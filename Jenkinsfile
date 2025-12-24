pipeline {
  agent any

  environment {
    IMAGE_NAME = "ci-cd-demo"
    CONTAINER_NAME = "ci-cd-demo-app"
  }

  stages {
    stage('Build & Test') {
      steps {
        bat 'npm run build'
        bat 'npm run test'
      }
    }

    stage('Docker Build') {
      steps {
        bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
        bat 'docker tag %IMAGE_NAME%:%BUILD_NUMBER% %IMAGE_NAME%:latest'
      }
    }

    stage('Deploy (Docker Run)') {
      steps {
        bat '''
          docker rm -f %CONTAINER_NAME% 2>nul || exit /b 0
          docker run -d --name %CONTAINER_NAME% -p 3000:3000 %IMAGE_NAME%:latest
        '''
      }
    }
  }
}

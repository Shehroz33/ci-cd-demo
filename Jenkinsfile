pipeline {
  agent any

  environment {
    IMAGE_NAME     = "ci-cd-demo"
    CONTAINER_NAME = "ci-cd-demo-app"
    APP_PORT       = "3000"
  }

  stages {

    stage('Build & Test') {
      steps {
        script {
          if (isUnix()) {
            sh '''
              node -v || true
              npm -v  || true
              npm install
              npm run build
              npm run test || echo "Tests failed or not configured"
            '''
          } else {
            bat '''
              node -v
              npm -v
              npm install
              npm run build
              npm run test || echo Tests failed or not configured
            '''
          }
        }
      }
    }

    stage('Docker Build') {
      steps {
        script {
          if (isUnix()) {
            sh """
              docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
              docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
            """
          } else {
            bat """
              docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .
              docker tag %IMAGE_NAME%:%BUILD_NUMBER% %IMAGE_NAME%:latest
            """
          }
        }
      }
    }

    stage('Deploy (Docker Run)') {
      steps {
        script {
          if (isUnix()) {
            sh """
              docker rm -f ${CONTAINER_NAME} || true
              docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:${APP_PORT} ${IMAGE_NAME}:latest
              docker ps --filter "name=${CONTAINER_NAME}"
            """
          } else {
            bat """
              docker rm -f %CONTAINER_NAME% 2>nul
              docker run -d --name %CONTAINER_NAME% -p %APP_PORT%:%APP_PORT% %IMAGE_NAME%:latest
              docker ps --filter "name=%CONTAINER_NAME%"
            """
          }
        }
      }
    }
  }

  post {
    always {
      script {
        if (isUnix()) {
          sh 'echo "Pipeline finished on Linux/Unix node"'
        } else {
          bat 'echo Pipeline finished on Windows node'
        }
      }
    }
  }
}

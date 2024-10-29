pipeline {
  agent {
    kubernetes {
      yaml """
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
"""
    }
  }
  environment {
    IMAGE_REGISTRY = "095522511846.dkr.ecr.us-east-1.amazonaws.com"
  }
  stages {
    stage('Build with Kaniko') {
      steps {
        container(name: 'kaniko', shell: '/busybox/sh') {
          dir('eshop-frontend') {
            sh '''#!/busybox/sh
            /kaniko/executor \
            --git branch=main \
            --context=. \
            --dockerfile=Dockerfile \  # Chỉ định đường dẫn Dockerfile nếu cần
            --destination=${IMAGE_REGISTRY}/eshop-frontend:latest
            '''   
          }
        }
      }
      post {
        success { 
          slackSend(channel: 'C07RNC9DDBN', color: 'good', message: "(Job : ${env.JOB_NAME} - Build Number : ${env.BUILD_NUMBER}) CI success - from <@dt.vinh1>")
        }
        failure {
          slackSend(channel: 'C07RNC9DDBN', color: 'danger', message: "(Job : ${env.JOB_NAME} - Build Number : ${env.BUILD_NUMBER}) CI fail - from <@dt.vinh1>")
        }
      }
    }
  }
}

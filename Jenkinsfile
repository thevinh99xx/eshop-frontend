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
            --destination=${IMAGE_REGISTRY}/eshop-frontend:latest
            '''   
          }
        }
      }
    }
  }
}

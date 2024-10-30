/*
<< 변수 >> 치환 필요

<< ECR URI >>      => ex) 123456789012.dkr.ecr.us-east-1.amazonaws.com
<< TAG >>          => ex) latest
<< CHANNEL ID >>   => Slack 교재-정보공유 채널에 공유된 ci-notice 채널의 ID 값으로 변경
<< MEMBER ID >>       => Slack 워크스페이스 내 개인별 멤버 ID 값으로 변경
*/
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
            sh '''#!/busybox/sh
            /kaniko/executor \
            --git branch=main \
            --context=. \
            --destination=${IMAGE_REGISTRY}/eshop-frontend:latest
        }
      }
      post {
        success { 
          slackSend(channel: 'C07RNC9DDBN', color: 'good', message: "(Job : ${env.JOB_NAME} - Build Number : ${env.BUILD_NUMBER}) CI success - from <@U07RQ3C04LT>")
        }
        failure {
          slackSend(channel: 'C07RNC9DDBN', color: 'danger', message: "(Job : ${env.JOB_NAME} - Build Number : ${env.BUILD_NUMBER}) CI fail - from <@U07RQ3C04LT>")
        }
      }
    }
  }
}

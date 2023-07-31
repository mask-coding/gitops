pipeline {
  agent any
  stages {
    stage('deploy start') {
      steps { //+추가됨: 배포 작업 이전에 배포 시작 알림 메시지를 슬랙 채널로 보냄
        slackSend(message: "Deploy ${env.BUILD_NUMBER} Started"
        , color: 'good', tokenCredentialId: 'slack-key')
      }
    }      

    stage('git pull') {
      steps {
        // https://github.com/mask-coding/gitops.git will replace by sed command before RUN
        git url: 'https://github.com/mask-coding/gitops.git', branch: 'main'
      }
    }
    stage('k8s deploy'){
      steps {
        withKubeConfig([credentialsId: 'cp-k8s', serverUrl: 'https://192.168.1.10:6443']) {
          sh 'kubectl apply -f deployment.yaml'
        }
      }
    }

    stage('deploy end') {
      steps { //+추가됨: 배포 작업 이후에 배포 완료 알림 메시지를 슬랙 채널로 보냄
        slackSend(message: """${env.JOB_NAME} #${env.BUILD_NUMBER} End
        """, color: 'good', tokenCredentialId: 'slack-key')
      }
    }

  }
}

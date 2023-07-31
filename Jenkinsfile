pipeline {
  agent any
  stages {
    stage('deploy start') {
      steps {
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

    stage('send diff') {
      steps {
        script { //+추가됨: 이전 배포와 현재 배포의 코드 변동 사항을 html로 만듦
          def publisher = LastChanges.getLastChangesPublisher "PREVIOUS_REVISION", "SIDE", "LINE", true, true, "", "", "", "", ""
          publisher.publishLastChanges()
          def htmlDiff = publisher.getHtmlDiff()
          writeFile file: "deploy-diff-${env.BUILD_NUMBER}.html", text: htmlDiff
        } // +추가됨: 변경사항을 한눈에 확인할 수 있는 주소를 메시지로 전달
        slackSend(message: """${env.JOB_NAME} #${env.BUILD_NUMBER} End
        (<${env.BUILD_URL}/last-changes|Check Last changed>)"""
        , color: 'good', tokenCredentialId: 'slack-key')
      }
    }
  }
}

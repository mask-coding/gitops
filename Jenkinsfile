pipeline {
  agent any
  stages {
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
  }
}

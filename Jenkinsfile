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
        kubernetesDeploy(kubeconfigId: 'kubeconfig',
                         configs: '*.yaml')
      }
    }    
  }
}
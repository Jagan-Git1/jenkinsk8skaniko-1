pipeline {

  options {
    ansiColor('xterm')
  }

  agent {
    kubernetes {
      yamlFile 'builder.yaml'
    }
  }

  stages {

stage('Checkout Source') {
      steps {
        script {
                    git branch: 'kaniko',
                        credentialsId: 'jagan-git1',
                        url: 'https://github.com/Jagan-Git1/jenkinsk8skaniko-1.git'
                }
      }
    }
	
	
    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile \
                             --context `pwd` \
                             --destination=jagantata1/myweb:${BUILD_NUMBER}
            '''
          }
        }
      }
    }

    stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" myweb.yaml'
            sh 'kubectl apply -f myweb.yaml'
          }
        }
      }
    }
  
  }
}
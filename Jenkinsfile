pipeline {

  agent {
    kubernetes {
      yamlFile 'builder.yaml'
    }
  }

  stages {

    stage('Deploy App to Kubernetes') {
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'kubectl create ns crud3'
            sh 'kubectl apply -f ./manifests -n crud3'
          }
        }
      }
    }
    
    stage('Test'){
      steps{
        script{
          
         
          echo '-------------Link test started-------------'
         container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'kubectl get svc -n crud3'
            sh 'kubectl get po -n crud3'
            sh 'telnet 192.168.49.1 80'

          }
        }
          echo '-------------Link test finished-------------'
          
          }
        }
      }
    }

  
}


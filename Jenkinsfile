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
            sh 'kubectl create ns crud2'
            sh 'kubectl apply -f ./manifests -n crud2'
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
            sh 'kubectl get svc -n crud2'
            sh 'kubectl get po -n crud2'
            sh 'busybox-extras telnet 127.0.0.1 80'
            sh 'nc -vz 127.0.0.1 80'
            sh 'if[[$(nc -vz 127.0.0.1 80)=="Connection to 127.0.0.1 80 port [tcp/http] succeeded!"]];then echo "good connection"; else error;exit 1;fi'
          }
        }
          echo '-------------Link test finished-------------'
          
          }
        }
      }
    }

  
}


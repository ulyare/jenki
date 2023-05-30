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
            sh 'kubectl get ns'
            sh 'kubectl get svc -n crud2'
            sh 'kubectl get deployments -n crud2 | grep web | awk \'{print $1}\''
	    sh 'if [[ $(kubectl get deployments -n crud2 | grep web | awk \'{print $2}\') == "1/1" ]]; then echo \'web-server is ready\'; else error; exit 1; fi'
            sh 'curl -D - -s http://127.0.0.1:1500 | grep 200 | awk "{print $2}"'
          }
        }
          echo '-------------Link test finished-------------'
          
          }
        }
      }
    }

  
}

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
          echo '-------------Build number test started-------------'
          echo 'Job Name: ' + env.JOB_NAME
          if (Integer.parseInt(env.BUILD_NUMBER) > 10) {
            echo 'Build number over 10'
            echo 'Node name: ' + env.NODE_NAME
            echo 'Workspace: ' + env.WORKSPACE
            echo 'Job url: ' + env.JOB_URL
            echo 'Jenkins url: ' + env.JENKINS_URL
          }
          else {
            sh "echo 'Tasks ran elsewhere'"
          }
          echo '-------------Build number test finished-------------'

          echo '-------------Link test started-------------'
         container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'kubectl get ns'
            sh 'kubectl get svc -n crud2'
            sh 'kubectl get deployments -n crud2 | grep web | awk \'{print $1}\''
	          sh 'if [[ $(kubectl get deployments -n crud2 | grep web | awk \'{print $2}\') == "1/1" ]]; then echo \'web-server is ready\'; else error; exit 1; fi'
            text = sh 'kubectl get po -n crud2'
          }
        }
          echo '-------------Link test finished-------------'
          
          }
        }
      }
    }

  
}

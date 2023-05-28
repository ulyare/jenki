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
          echo '-------------Test started-------------'
          echo 'Job Name: ' + env.JOB_NAME
          if (env.JOB_NAME == 'sunnyday') {
            echo 'Tasks ran on the sunnyday job'
            echo env.NODE_NAME
            echo env.NODE_LABEL
            echo env.WORKSPACE
            echo env.JOB_URL
            echo env.JENKINS_URL
            
            server_ip="env.JENKINS_URL"
            SYS_PING="ping -c 5 ${server_ip} | grep 'received' | awk -F',' '{print \$2}'"
            echo "Pinging ${server_ip} with response: "${SYS_PING}" 
          }
          else {
            sh "echo 'Tasks ran elsewhere'"
          }
          echo '-------------Test finished-------------'
        }
      }
    }

  }
}

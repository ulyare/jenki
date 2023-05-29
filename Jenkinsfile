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
          if (Integer.parseInt(env.BUILD_NUMBER) > 10) {
            echo 'Build number over 10'
            echo 'Node name: ' + env.NODE_NAME
            echo 'Workspace: ' + env.WORKSPACE
            echo 'Job url: ' + env.JOB_URL
            echo 'Jenkins url' + env.JENKINS_URL
          }
          else {
            sh "echo 'Tasks ran elsewhere'"
          }
          echo '-------------Test finished-------------'
          @Name("Search form")
          @FindBy(xpath = "//form")
          public class SearchArrow extends HtmlElement {

            @Name("Search request input")
            @FindBy(id = "searchInput")
            private TextInput requestInput;

            @Name("Search button")
            @FindBy(className = "b-form-button__input")
            private Button searchButton;

            public void search(String request) {
              requestInput.sendKeys(request);
              searchButton.click();
             }
           }
          if(get(env.JENKINS_URL)) {
            echo 'server is available'
          }
          else {
            echo 'server is not available'
          }
        }
      }
    }

  }
}

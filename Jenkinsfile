pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/vamsijakkula/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                   sh "docker build -t testapps:latest . "
                   sh "docker tag testapps:latest 10.10.10.52:8123/testapps:latest"
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                  withCredentials([string(credentialsId: 'DockerRegistryPWD', variable: 'DockerRegistryPWD')]) {
                  sh "docker login -u admin -p '${DockerRegistryPWD}' 10.10.10.52:8123"
                  } 
                  sh "docker push 10.10.10.52:8123/testapps:latest"
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "KUBERNETES_CLUSTER_CONFIG")
        }
      }
    }

  }

}

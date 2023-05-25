pipeline {

  environment {
    dockerimagename = "nandhini1618/nodeapp"
    dockerImage = ""
  }

  agent default-242fd
  tools {
    dockerTool "docker"
  }
  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/Nandhini-16/docker-node-pipeline.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}

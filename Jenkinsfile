 pipeline {
  environment {
      registry = "172.18.0.7:8081"
      dockerRegistry = "172.18.0.7:8081"
      registryCredential = 'admin'
      ImageName = "example"
      ImageTag= "01"
      dockerImage = ''
      gitRepositoryUrl = "https://github.com/ederbritodf/example.git"
      gitCredential = "ederbritodf"
      gitBranch = "master"
  }

agent any

stages {
  stage('Checkout Código-fonte') {
      steps {
        git([
            poll: true,
            credentialsId: gitCredential,
            url: gitRepositoryUrl,
            branch: gitBranch
        ])

    }
  }
  
      stage('Building image') {
        steps{
          script {
            dockerImage = docker.build registry + "/$ImageName:$ImageTag"
          }
        }
      }
      stage('Deploy Image') {
        steps{
          script {
              docker.withRegistry( 'http://$dockerRegistry/', 'docker-registry' ) {
                  docker.build(ImageName)
                      .push(ImageTag)
            }
          }
        }
      }
      stage('Remove Unused docker image') {
        steps{
          sh "docker rmi $registry/$ImageName:$ImageTag"
        }
      }
    }
  }

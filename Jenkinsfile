pipeline {
  agent any
  stages {
    stage('Building image') {
      steps {
        withSonarQubeEnv(installationName: 'Sonar', credentialsId: 'Sonar', envOnly: true) {
          sh 'echo "Preparing sonar"'
          waitForQualityGate(credentialsId: 'Sonar', webhookSecretId: 'Sonar', abortPipeline: true)
        }

        script {
          dockerImage = docker.build registry
        }

      }
    }

    stage('docker stop container') {
      steps {
        sh 'docker ps -f name=mypythonappContainer -q | xargs --no-run-if-empty docker container stop'
        sh 'docker container ls -a -fname=mypythonappContainer -q | xargs -r docker container rm'
      }
    }

    stage('Docker Run') {
      steps {
        script {
          dockerImage.run("-p 8096:5000 --rm --name mypythonappContainer")
        }

      }
    }

  }
  environment {
    registry = 'your_docker_user_id/mypythonapp'
    registryCredential = 'fa32f95a-2d3e-4c7b-8f34-11bcc0191d70'
    dockerImage = ''
  }
}
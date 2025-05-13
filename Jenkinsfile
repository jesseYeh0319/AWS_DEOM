pipeline {
  agent any

  environment {
    REMOTE_HOST = "35.77.89.158"
    REMOTE_USER = "ubuntu"
    REMOTE_PATH = "/home/ubuntu/deploy/app"
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('Deploy') {
      steps {
        sshagent(['ec2-ssh-key']) {
          sh """
            scp -o StrictHostKeyChecking=no target/*.jar ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/
          """
        }
      }
    }
  }
}

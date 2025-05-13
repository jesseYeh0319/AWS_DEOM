pipeline {
  agent any

  environment {
    REMOTE_HOST = "ec2-54-206-91-241.ap-southeast-2.compute.amazonaws.com"
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

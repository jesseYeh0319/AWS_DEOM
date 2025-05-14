pipeline {
  agent any

  environment {
    REMOTE_USER = 'ubuntu'
    REMOTE_HOST = 'ec2-3-107-47-79.ap-southeast-2.compute.amazonaws.com'
    SSH_KEY_ID = 'ec2-ssh-key'
  }

  stages {
    stage('打包') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('上傳 JAR') {
      steps {
        sshagent (credentials: ["${SSH_KEY_ID}"]) {
          sh '''
            scp -o StrictHostKeyChecking=no target/app.jar ${REMOTE_USER}@${REMOTE_HOST}:/home/ubuntu/app.jar
          '''
        }
      }
    }

    stage('遠端重啟服務') {
      steps {
        sshagent (credentials: ["${SSH_KEY_ID}"]) {
          sh '''
            ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} 'bash /home/ubuntu/restart.sh'
          '''
        }
      }
    }
  }
}
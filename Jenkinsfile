pipeline {
  agent any
  tools {
    maven 'Maven 3'    // name you assign in Global Tool Config
    jdk 'JDK21'   // name you assign for JDK 17
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests=false clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit '**/target/surefire-reports/*.xml'
        }
      }
    }
    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
    stage('Deploy') {
      steps {
        // Example: SCP to remote server using SSH key stored in credentials
        withCredentials([sshUserPrivateKey(credentialsId: 'deploy-key', keyFileVariable: 'SSH_KEY')]) {
          sh 'scp -i $SSH_KEY target/*.jar deployuser@yourserver:/opt/apps/'
        }
        // OR: run your deploy script: sh './deploy/deploy-to-prod.sh'
      }
    }
  }
  post {
    success { echo 'Build succeeded' }
    failure { echo 'Build failed' }
  }
}

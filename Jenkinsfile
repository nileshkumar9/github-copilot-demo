pipeline {
  agent any

  tools {
    maven 'mvn'
  }

  stages {
    stage('Build App') {
      steps {
        sh 'mvn package'
      }
    }
    
    stage('Build Image') {
      steps {
      sh "docker build -t nilesh35apiit/github-copilot-demo:${env.BUILD_ID} ."
      sh "docker tag nilesh35apiit/github-copilot-demo:${env.BUILD_ID} nilesh35apiit/github-copilot-demo:latest"
      }
    }
    
  }

  post {
    always {
      archive 'target/**/*.jar'
    }
    success {
      withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
        sh "docker push nilesh35apiit/github-copilot-demo:${env.BUILD_ID}"
        sh "docker push nilesh35apiit/github-copilot-demo:latest"
      }
    }
  }
}

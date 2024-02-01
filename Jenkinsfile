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
      sh "docker build -t nileshkumar9/github-copilot-demo:${env.BUILD_ID} ."
      sh "docker tag nileshkumar9/github-copilot-demo:${env.BUILD_ID} nileshkumar9/github-copilot-demo:latest"
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
        sh "docker push nileshkumar9/github-copilot-demo:${env.BUILD_ID}"
        sh "docker push nileshkumar9/github-copilot-demo:latest"
      }
    }
  }
}

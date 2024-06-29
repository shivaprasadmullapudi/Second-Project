pipeline {
  agent {
    docker {
      image 'abhishekf5/maven-abhishek-docker-agent:v1'
      args '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemon
    }
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('shivajenkins')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t shivaprasadmullapudi359/dockerrepo:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push shivaprasadmullapudi359/dockerrepo:latest'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}

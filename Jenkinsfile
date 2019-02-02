pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /?:/? -u root'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'cd /'
        sh 'ls -la'
        sh 'whoami'
        sh 'mvn help:evaluate -Dexpression=settings.localRepository'
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }
  }
}
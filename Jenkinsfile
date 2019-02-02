pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /jenkins/.m2:/jenkins/.m2 -u jenkins'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'whoami'
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
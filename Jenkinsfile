pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /jenkins/.m2:/jenkins/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }
  }
  environment {
    MAVEN_OPTS = '-Duser.home=/jenkins'
  }
}
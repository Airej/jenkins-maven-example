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
        junit 'target/surefire-reports/*.xml'
      }
    }
    stage('Static Tests') {
      steps {
        sh 'mvn site'
        publishHTML(target: [
          allowMissing: false,
          alwaysLinkToLastBuild: false,
          keepAll: true,
          reportDir: 'target/site',
          reportFiles: 'checkstyle.html',
          reportTitles: "Checkstyle",
          reportName: "CheckStyle Report"
        ])
      }
    }
  }
  environment {
    MAVEN_OPTS = '-Duser.home=/jenkins'
  }
}

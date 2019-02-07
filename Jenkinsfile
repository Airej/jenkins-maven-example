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
        sh 'mvn pmd:cpd'
        sh 'mvn pmd:pmd'
        sh 'mvn compile site'
        recordIssues enabledForFailure: true, tool: checkStyle(pattern: 'target/checkstyle-result.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: cpd(pattern: 'target/cpd.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: pmdParser(pattern: 'target/pmd.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: spotBugs(pattern: 'target/spotbugsXml.xml'), sourceCodeEncoding: 'UTF-8'
      }
      post{
        always{
          step([$class: 'CoberturaPublisher',
                         autoUpdateHealth: false,
                         autoUpdateStability: false,
                         coberturaReportFile: 'target/site/cobertura/coverage.xml',
                         failNoReports: false,
                         failUnhealthy: false,
                         failUnstable: false,
                         maxNumberOfBuilds: 10,
                         onlyStable: false,
                         sourceEncoding: 'ASCII',
                         zoomCoverageChart: false])
        }
      }
    }
  }
  environment {
    MAVEN_OPTS = '-Duser.home=/jenkins'
  }
}

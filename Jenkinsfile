pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-u root -v /jenkins/.m2:/jenkins/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -U -B -DskipTests clean package'
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
        // Run all the static tests (style, CPD (=copy-paste detector), PMD (=Programming Mistake Detector) and spotBugs, coverage)
        // sh 'mvn compile site'
        sh 'mvn pmd:pmd'
        sh 'mvn pmd:cpd'
        // Send the results to Jenkins
        // recordIssues enabledForFailure: true, tool: checkStyle(pattern: 'target/checkstyle-result.xml'), sourceCodeEncoding: 'UTF-8'
        // recordIssues enabledForFailure: true, tool: cpd(pattern: 'target/cpd.xml'), sourceCodeEncoding: 'UTF-8'
        // recordIssues enabledForFailure: true, tool: pmdParser(pattern: 'target/pmd.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: spotBugs(pattern: 'target/spotbugsXml.xml'), sourceCodeEncoding: 'UTF-8'
      }
      // Send Coverage results
      // post{
      //   always{
      //     step([$class: 'CoberturaPublisher',
      //                    autoUpdateHealth: false,
      //                    autoUpdateStability: false,
      //                    coberturaReportFile: 'target/site/cobertura/coverage.xml',
      //                    failNoReports: false,
      //                    failUnhealthy: false,
      //                    failUnstable: false,
      //                    maxNumberOfBuilds: 10,
      //                    onlyStable: false,
      //                    sourceEncoding: 'ASCII',
      //                    zoomCoverageChart: false])
      //   }
      // }
    }
  }
  environment {
    MAVEN_OPTS = '-Duser.home=/jenkins'
  }
}

// pipeline {
//     agent {
//         docker {
//             image 'maven:3-alpine'
//             args '-v /jenkins/.m3:/jenkins/.m3'
//         } 
//     }
//     stages {
//         stage('Build') {
//             steps {
//                 // Run Maven build with full stack trace and debug logging
//                 sh 'mvn -e -X -B -DskipTests clean package'
//             }
//         }
//         stage('Test') {
//             steps {
//                 // Run Maven test with full stack trace and debug logging
//                 sh 'mvn -e -X test'
//             }
//         }
//         stage('Static Tests') {
//             steps {
//                 // Run Maven static tests with full stack trace and debug logging
//                 sh 'mvn -e -X compile site'
//                 sh 'mvn -e -X pmd:pmd'
//                 sh 'mvn -e -X pmd:cpd'
//                 // Send the results to Jenkins
//                 recordIssues enabledForFailure: true, tool: checkStyle(pattern: 'target/checkstyle-result.xml'), sourceCodeEncoding: 'UTF-8'
//                 recordIssues enabledForFailure: true, tool: cpd(pattern: 'target/cpd.xml'), sourceCodeEncoding: 'UTF-8'
//                 recordIssues enabledForFailure: true, tool: pmdParser(pattern: 'target/pmd.xml'), sourceCodeEncoding: 'UTF-8'
//                 recordIssues enabledForFailure: true, tool: spotBugs(pattern: 'target/spotbugsXml.xml'), sourceCodeEncoding: 'UTF-8'
//             }
//             post {
//                 always {
//                     step([$class: 'CoberturaPublisher',
//                         autoUpdateHealth: false,
//                         autoUpdateStability: false,
//                         coberturaReportFile: 'target/site/cobertura/coverage.xml',
//                         failNoReports: false,
//                         failUnhealthy: false,
//                         failUnstable: false,
//                         maxNumberOfBuilds: 10,
//                         onlyStable: false,
//                         sourceEncoding: 'ASCII',
//                         zoomCoverageChart: false])
//                 }
//             }
//         }
//     }
//     environment {
//         MAVEN_OPTS = '-Duser.home=/jenkins'
//     }
// }

pipeline {
  agent any

  environment {
    MAJOR_VERSION = 1
  }

  stages {
    stage('build') {
      steps {
        sh 'ant -f build.xml -v'
      }
    }
    stage('Unit Tests') {
      agent {
        label 'apache'
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
  }
  post {
    always    {
        archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}

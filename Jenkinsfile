pipeline {
  agent any

  environment {
    MAJOR_VERSION = 1
    BRANCH_NAME = 'development'
  }

  stages {
    stage('build') {
      steps {
        sh 'ant -f build.xml -v'
      }
    }
    stage('Unit Tests') {
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('deploy') {
      steps {
        sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
      }
    }
  }
  post {
    always    {
        archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}

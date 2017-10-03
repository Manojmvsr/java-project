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
        sh "if ![ -d '/var/www/html/rectangles/all/${env.BRANCH_NAME}' ]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}; fi"
        sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
      }
    }
  }
  post {
    always    {
        archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}

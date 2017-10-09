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
    stage("Test on Debian") {
      agent {
        docker 'openjdk:8u121-jre'
      }
      steps {
        sh "java -version"
      }
    }
    stage('Promote to Green') {
      when {
        branch 'master'
      }
      steps {
        sh "cp /var/www/html/rectangles/all/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar"
      }
    }
    stage('Promote Development Branch to Master') {
      when {
        branch 'development'
      }
      steps {
        echo "Stashing Any Local Changes"
        sh 'git stash'
        echo "Checking Out Development Branch"
        sh 'git checkout development'
        echo 'Checking Out Master Branch'
        sh 'git pull origin'
        sh 'git checkout master'
        echo 'Merging Development into Master Branch'
        sh 'git merge development'
        echo 'Pushing to Origin Master'
        sh 'git push origin master'
        echo 'Tagging the Release'
        sh "git tag rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
        sh "git push origin rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
      }
  }
  post {
    always    {
        archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
}
}

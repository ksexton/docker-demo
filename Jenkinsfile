#!groovy
pipeline {
  agent {
    label 'docker'
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        script {
          myBuild = docker.build("ksexton/docker-demo:${env.BUILD_ID}")
        }
      }
    }
    stage('Test') {
      steps {
        script {
          myBuild.inside("-v ${pwd()}:/build") {
            sh 'echo "Inside container running tests."'
          }
        }
      }
    }
    stage('Documentation') {
      when {
        branch 'master'
      }
      steps {
        script {
          myBuild.inside("-v ${pwd()}:/build") {
            sh 'echo "Inside container building documentation"'
            sh 'cd /build/tests; python -m pytest test_capitalize.py'
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        echo "Here we would deploy"
        // docker.withRegistry('https://registry.example.com', 'credentials-id') {
        //   myBuild.push()
        // }
      }
    }
  }
}

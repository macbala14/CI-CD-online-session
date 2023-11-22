pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }

    stage('Unit-Test') {
      steps {
        script {
          docker.image("${registry}:${env.BUILD_ID}").inside{
            c-> sh 'python app_test.py'}
          }

        }
      }

    }
    environment {
      registry = 'mbala14/postgrad-test'
    }
  }
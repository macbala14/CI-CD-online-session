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
    stage('http-test') {
      steps {
        script{
          docker.image("${registry}:${env.BUILD_ID}").with('-p 9005:9000'){
            c -> sh "sleep5; curl -i http://localhost:9005/test_string"
          }
        }
      }
    }
  }
    environment {
      registry = 'mbala14/postgrad-test'
    }
  }

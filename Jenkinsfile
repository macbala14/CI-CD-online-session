pipeline {
  agent any
  stages {
    stage('Check Code Quality'){
      steps{
        script {
          docker.image('python:3.9').inside{ c->
            sh'''
            python -m venv .venv
            . .venv/bin/activate
            pip install pylint
            pip install -r requirements.txt
            pylint --exit-zero --report=y --output-format=json:pylint-report.json,colorized ./*.py
            '''
          publishHTML target : [
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: './',
                    reportFiles: 'pylint-report.json',
                    reportName: 'pylint Scan',
                    reportTitles: 'pylint Scan'
                ]
          }
        }
      }
    }
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
          docker.image("${registry}:${env.BUILD_ID}").withRun('-p 9005:9000'){
            c -> sh "sleep 5; curl -i http://localhost:9005/test_string"
          }
        }
      }
    }
  }
    environment {
      registry = 'mbala14/postgrad-test'
    }
  }

pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Build') {
      parallel {
        stage('Install Deps') {
          steps {
            bat(script: 'SET CI=true && npm install', returnStatus: true)
          }
        }
        stage('Build Project') {
          steps {
            bat(script: 'SET CI=true && npm run build', returnStatus: true)
          }
        }
        stage('Unit Test') {
          steps {
            bat(script: 'SET CI=true && npm run test', returnStatus: true)
          }
        }
      }
    }
    stage('Release') {
      steps {
        powershell(script: '//jenkins-build/add-config.ps1', returnStatus: true)
        bat(script: 'aws sync -r build', returnStatus: true)
      }
    }
    stage('Run') {
      steps {
        echo 'Build %RELEASE_VERSION% available to use'
      }
    }
  }
  environment {
    ENV_NAME = 'Dev'
    WEB_API = '""'
  }
}
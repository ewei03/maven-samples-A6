pipeline {
  agent any

  options {
    skipDefaultCheckout(true)
  }

  tools {
    maven 'mvn'
    jdk 'jdk'
  }

  environment {
    GOOD_COMMIT = '98ac319c0cff47b4d39a1a7b61b4e195cfa231e5'
    BAD_COMMIT  = '198644632661c67b6c32f59e9047c11a70685e15'
  }

  stages {
    stage('check out') {
      steps {
        deleteDir()
        bat '''
git clone https://github.com/ewei03/maven-samples.git .
git checkout master
'''
      }
    }

    stage('prepare bisect script') {
      steps {
        writeFile file: 'bisect_test.bat', text: '''@echo off
call mvn clean test verify
exit /b %ERRORLEVEL%
'''
      }
    }

    stage('run bisect') {
      steps {
        bat '''
git bisect reset
git bisect start %BAD_COMMIT% %GOOD_COMMIT%
git bisect run cmd /c .\\bisect_test.bat
'''
      }
    }
  }

  post {
    always {
      bat 'git bisect reset'
    }
  }
}

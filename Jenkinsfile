pipeline {
  agent any
  stages {
    stage('check out') {
      steps {
        git(url: 'https://github.com/ewei03/maven-samples', branch: 'master')
      }
    }

    stage('prepare bisect script') {
      steps {
        writeFile(file: 'bisect_test.bat', text: '''@echo off
call mvn test
exit /b %ERRORLEVEL%
''')
      }
    }

    stage('git bisect') {
      steps {
        bat '''
git bisect reset
git bisect start 198644632661c67b6c32f59e9047c11a70685e15 98ac319c0cff47b4d39a1a7b61b4e195cfa231e5
git bisect run bisect_test.bat
'''
      }
    }

  }
  tools {
    maven 'mvn'
    jdk 'jdk'
  }
  post {
    always {
      bat 'git bisect reset'
    }

  }
}
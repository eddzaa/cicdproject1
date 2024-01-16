def registry = 'https://bkrraj.jfrog.io'
pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'master', url: 'https://github.com/eddzaa/cicdproject1.git'
      }
    }
 stage('Pull Changes') {
      steps {
        sh 'git pull origin master'
        echo "WebHook Is Successfull Change"
      }
      }

stage ('Build') {
          steps {

            sh 'mvn clean install'    

            }
      }

   stage('Unit Test') {
      steps {
        echo '<--------------- Unit Testing started  --------------->'
        sh 'mvn surefire-report:report'
        echo '<------------- Unit Testing stopped  --------------->'
      }
    }


stage('Sonar Analysis') {
      environment {
        scannerHome = tool 'SonarQubeScanner'
      }
      steps {
        echo '<--------------- Sonar Analysis started  --------------->'
                withSonarQubeEnv('SonarQubeScanner') {
                sh "${scannerHome}/bin/sonar-scanner"
                }       
         }
      }
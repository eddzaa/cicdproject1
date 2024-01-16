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
      stage('Quality Gate') {
      steps {
        script {
          echo '<--------------- Quality Gate started  --------------->'
          timeout(time: 1, unit: 'MINUTES') {
            def qg = waitForQualityGate()
            if (qg.status != 'OK') {
              error 'Pipeline failed due to the Quality gate issue: ${qg.status}'
            }
          }
          echo '<--------------- Quality Gate stopped  --------------->'
        }
      }
    }
    stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t myrepo .'
                }
            }
    }
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 623231584756.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker tag myrepo:latest 623231584756.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
                sh 'docker push 623231584756.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest'
         }
        }
      }
  }
}
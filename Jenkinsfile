pipeline {
  agent {
    docker {
      image 'node'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          environment {
            CI = 'true'
          }
          steps {
            sh '''git pull
chmod 766 ./jenkins/scripts/test.sh
./jenkins/scripts/test.sh'''
          }
        }
        stage('Before Test') {
          steps {
            echo 'Before Test'
          }
        }
      }
    }
    stage('Deliver') {
      steps {
        sh '''git pull
chmod 766 ./jenkins/scripts/deliver.sh
./jenkins/scripts/deliver.sh'''
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh '''git pull
chmod 766 ./jenkins/scripts/kill.sh
./jenkins/scripts/kill.sh'''
      }
    }
  }
}
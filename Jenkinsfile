pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('deploy') {
      steps {
        sh 'scp target/*.jar jenkins@192.168.94.119:/home/jenkins'
      }
    }
  }
}
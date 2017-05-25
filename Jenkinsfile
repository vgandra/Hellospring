pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('test') {
      steps {
        sh '''mvn clean verify
mvn sonar:sonar'''
      }
    }
    stage('deploy') {
      steps {
        sh 'cp target/*.jar /opt/deploy'
      }
    }
  }
}
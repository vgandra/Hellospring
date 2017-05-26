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
        parallel(
          "deploy": {
            sh 'scp target/*.jar jenkins@192.168.94.119:/home/jenkins'
            
          },
          "artifacts": {
            archiveArtifacts(artifacts: 'target/*.jar', allowEmptyArchive: true, caseSensitive: true, defaultExcludes: true)
            
          }
        )
      }
    }
    stage('') {
      steps {
        emailext(subject: 'hello', body: 'hello', attachLog: true, compressLog: true, to: 'sgandra@altimetrik.com')
      }
    }
  }
}
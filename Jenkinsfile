node {
  checkout scm
  env.PATH = "${tool 'Maven3'}/bin:${env.PATH}"
  stage('Package') {
       bat 'mvn clean package -DskipTests'
    }
}
node {
  checkout scm
  env.PATH = "${tool 'Maven3'}/bin:${env.PATH}"
  
  stage('Validate') {
       bat 'mvn validate'
    }
	
  stage('Compile') {
       bat 'mvn compile'
    }	
  stage('Package') {
       bat 'mvn clean package -DskipTests'
    }
	
  stage ('test'){
	parallel 'integration': {	
		bat "mvn clean verify" 			
	}, 'quality': {
	//	bat "mvn sonar:sonar" 		
	}
	}
	
	stage ('approve')
	timeout(time: 7, unit: 'DAYS') {
		input message: 'Do you want to deploy?', submitter: 'ops'
	}
	
	stage name:'deploy', concurrency: 1{
				bat "mvn cargo:deploy" 
			}
}
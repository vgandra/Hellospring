node {
  checkout scm
  env.PATH = "${tool 'Maven3'}/bin:${env.PATH}"
  stash excludes: 'target/', includes: '**', name: 'source'
  
  stage('validate') {
       bat 'mvn validate'
    }
	
  stage('compile') {
       bat 'mvn compile'
    }	
  stage('package') {
       bat 'mvn clean package -DskipTests'
    }
	
  stage('install') {
       bat 'mvn clean install'
    }	
	
  stage ('test'){
	parallel 'integration': {	
		bat "mvn clean verify" 			
	}, 'quality': {
	//	bat "mvn sonar:sonar" 		
	}
	}
	
	//stage ('approve')
	//timeout(time: 7, unit: 'DAYS') {
	//	input message: 'Do you want to deploy?', submitter: 'ops'
	//}	
	
	stage ('deploy'){ 
	//			bat "mvn cargo:deploy" 
				 unstash 'source'
				bat "copy .\\target\\*.jar d:\\deploy"
	}	
}
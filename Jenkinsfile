node { 
	checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git-credentials', url: 'https://github.com/shyamnarayan2001/HelloSpringWorld.git']]])
	env.PATH ="${tool 'Maven3'}/bin:${env.PATH}"
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
	stage('test') {
		parallel 'integration': {
			bat 'mvn clean verify'
		}, 'quality': {
			//bat 'mvn sonar:sonar'
			} 
	} 
	stage('deploy') {
		unstash 'source'
		bat 'copy .\\target\\*.jar d:\\deploy'
	}
}
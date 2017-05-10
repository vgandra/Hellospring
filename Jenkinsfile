try {
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
			junit '**/target/*.xml'
		}, 'quality': {
			//bat 'mvn sonar:sonar'
			} 
	} 
	stage('deploy') {
		unstash 'source'
		bat 'copy .\\target\\*.jar d:\\deploy'
	} 
}
} // try end
catch (exc) {
/*
 err = caughtError
 currentBuild.result = "FAILURE"
 String recipient = 'jjeyashree@gmail.com'
 mail subject: "${env.JOB_NAME} (${env.BUILD_NUMBER}) failed",
         body: "It appears that ${env.BUILD_URL} is failing, somebody should do something about that",
           to: recipient,
      replyTo: recipient,
 from: 'noreply@ci.jenkins.io'
*/
} finally {
  
 (currentBuild.result != "ABORTED") && node("master") {
     // Send e-mail notifications for failed or unstable builds.
     // currentBuild.result must be non-null for this step to work.
     step([$class: 'Mailer',
        notifyEveryUnstableBuild: true,
        recipients: "${email_to}",
        sendToIndividuals: true])
 }
 
 // Must re-throw exception to propagate error:
 if (err) {
     throw err
 }
}
node('master') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
		sh 'mvn sonar:sonar -Dsonar.host.url=http://35.173.232.10:9000 -Dsonar.login=d41367789d4095655cbcfb551aa34037663bc99e'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		sh 'cp target/*.war /opt/tomcat8/webapps'
	}
	stage ('Notification'){
		slackSend color: 'good', message: 'Deployment Sucessful'
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "anuj_sharma401@yahoo.com"
		    )
	}
}

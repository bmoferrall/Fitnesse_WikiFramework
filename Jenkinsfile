pipeline {
	agent any
	stages {
		stage('Configure Environment') {
			steps {
				sh 'printenv'
			}
		}
		stage('Run IOC Suite') {
			options { 
				timestamps() 
				timeout(time: 1, unit: 'HOURS')
			}
			steps {
				sh 'java -jar fitnesse-standalone.jar -d "./" -p 9090 -o -b TestSuite_Results.xml -c "FitNesse.CognitiveCitiesSuiteOfSuites.TestSuites.IocSuite.IocServicesSuite.MiscellaneousIocServices?suite&format=junit"'
			}
		}
		stage('Archive Results') {
			steps {
				junit 'TestSuite_Results.xml'
				archiveArtifacts 'TestSuite_Results.xml'
			}
		}
		stage('Email Results') {
			steps {
				emailext attachmentsPattern: 'TestSuite_Results.xml',
				from: 'admin@jenkins.com',
				to: 'mooreof@ie.ibm.com',
				subject: "Fitnesse suite complete: ${currentBuild.fullDisplayName}",
				body: "Check attached file for results"
			}
		}
	}
}

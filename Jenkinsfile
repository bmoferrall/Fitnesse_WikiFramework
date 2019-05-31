pipeline {
	agent any
	parameters {
		choice(name: "PROPERTYFILE", choices: ['dubperfwow2.properties', 'amir_profile3.properties'], description: "Choose a property file to use")
		choice(name: 'OUTPUTFORMAT', choices: ['xml', 'html'], description: 'Select output format')
	}
	stages {
		stage('Configure Environment') {
			steps {
				sh "/var/lib/jenkins/workspace/lib/configure_fitnesse_ioc.sh ${params.PROPERTYFILE}"
			}
		}
		stage('Run IOC Suite') {
			options { 
				timestamps() 
				timeout(time: 1, unit: 'HOURS')
			}
			steps {
				script {
					if (params.OUTPUTFORMAT == 'html') {
						sh 'java -jar fitnesse-standalone.jar -d "./" -p 9090 -o -b TestSuite_Results.html -c "FitNesse.CognitiveCitiesSuiteOfSuites.TestSuites.IocSuite.IocServicesSuite.SopServicesSuite?suite&format=html"'
					} else {
						sh 'java -jar fitnesse-standalone.jar -d "./" -p 9090 -o -b TestSuite_Results.xml -c "FitNesse.CognitiveCitiesSuiteOfSuites.TestSuites.IocSuite.IocServicesSuite.SopServicesSuite?suite&format=junit"'
					}
				}
			}
		}
		stage('Archive Results') {
			steps {
				script {
					if (params.OUTPUTFORMAT == 'xml') {
						junit 'TestSuite_Results.xml'
					}
					archiveArtifacts "TestSuite_Results.${params.OUTPUTFORMAT}"
				}
			}
		}
	}
	post {
		success {
			emailext attachmentsPattern: "TestSuite_Results.${params.OUTPUTFORMAT}",
			from: 'admin@jenkins.com',
			to: 'mooreof@ie.ibm.com',
			subject: "Fitnesse suite complete (success): ${currentBuild.fullDisplayName}",
			body: "Successful test execution. Check attached file for results "
		}
		failure {
			emailext attachmentsPattern: "TestSuite_Results.${params.OUTPUTFORMAT}",
			from: 'admin@jenkins.com',
			to: 'mooreof@ie.ibm.com',
			subject: "Fitnesse suite complete (fail): ${currentBuild.fullDisplayName}",
			body: "Test execution failed. Check attached file for results "
		}
	}
}

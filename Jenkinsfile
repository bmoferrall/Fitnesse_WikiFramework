pipeline {
	agent any
	parameters {
		file(name: "PROPERTYFILE", description: "Choose a property file to use")
		choice(name: 'OUTPUTFORMAT', choices: ['xml', 'html'], description: 'Select output format')
	}
	stages {
		stage('Configure Environment') {
			steps {
				script {
					if (params.PROPERTYFILE == null) {
						params.PROPERTYFILE = '/var/lib/jenkins/workspace/properties/dubperfwow2.properties'
					}
					sh "/var/lib/jenkins/workspace/lib/configure_fitnesse_ioc.sh ${params.PROPERTYFILE}"
				}
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
						sh 'java -jar fitnesse-standalone.jar -d "./" -p 9090 -o -b TestSuite_Results.html -c "FitNesse.CognitiveCitiesSuiteOfSuites.TestSuites.IocSuite.IocServicesSuite.MiscellaneousIocServices?suite&format=html"'
					} else {
						sh 'java -jar fitnesse-standalone.jar -d "./" -p 9090 -o -b TestSuite_Results.xml -c "FitNesse.CognitiveCitiesSuiteOfSuites.TestSuites.IocSuite.IocServicesSuite.MiscellaneousIocServices?suite&format=junit"'
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
		stage('Email Results') {
			steps {
				emailext attachmentsPattern: "TestSuite_Results.${params.OUTPUTFORMAT}",
				from: 'admin@jenkins.com',
				to: 'mooreof@ie.ibm.com',
				subject: "Fitnesse suite complete: ${currentBuild.fullDisplayName}",
				body: "Check attached file for results"
			}
		}
	}
}

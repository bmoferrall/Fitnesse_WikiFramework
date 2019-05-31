pipeline {
	agent any
	parameters {
		choice(name: "PROPERTYFILE", choices: ['dubperfwow2.properties', 'amir_profile3.properties'], description: "Choose a property file to use")
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
				sh 'java -jar fitnesse-standalone.jar -d "./" -p 9090 -o -b IocSuite_Results.xml -c "FitNesse.CognitiveCitiesSuiteOfSuites.TestSuites.IocSuite.IocServicesSuite.SopServicesSuite?suite&format=xml"'
				sh 'xmlto -x /var/lib/jenkins/workspace/lib/xsl_fitnesse.xsl html IocSuite_Results.xml --skip-validation'
				sh 'mv IocSuite_Results.proc IocSuite_Results_junit.xml'
			}
		}
		stage('Process Results') {
			steps {
				junit 'IocSuite_Results_junit.xml'
				archiveArtifacts "IocSuite_Results.xml, IocSuite_Results_junit.xml"
			}
		}
	}
	post {
		success {
			emailext attachmentsPattern: "IocSuite_Results.xml, IocSuite_Results_junit.xml",
			from: 'admin@jenkins.com',
			to: 'mooreof@ie.ibm.com',
			subject: "Fitnesse suite complete (success): ${currentBuild.fullDisplayName}",
			body: "Successful test execution. Check attached file for results\nEnvironment: ${params.PROPERTYFILE}"
		}
		failure {
			emailext attachmentsPattern: "IocSuite_Results.xml, IocSuite_Results_junit.xml",
			from: 'admin@jenkins.com',
			to: 'mooreof@ie.ibm.com',
			subject: "Fitnesse suite complete (fail): ${currentBuild.fullDisplayName}",
			body: "Test execution failed. Check attached file for results\nEnvironment: ${params.PROPERTYFILE}"
		}
	}
}

pipeline {
	agent any
	parameters {
		string(name: 'WEBSERVER', defaultValue: 'dubperfwow2-web.mul.ie.ibm.com', description: 'Host name of web server')
		string(name: 'ANASERVER', defaultValue: 'dubperfwow2-ana.mul.ie.ibm.com', description: 'Host name of ana server')
		string(name: 'APPSERVER', defaultValue: 'dubperfwow2-app.mul.ie.ibm.com', description: 'Host name of app server')
		string(name: 'DBSERVER', defaultValue: 'dubperfwow2-db.mul.ie.ibm.com', description: 'Host name of db server')
		string(name: 'WEBPORT', defaultValue: '443', description: 'Port number of IOC log in')
		string(name: 'DBPORT', defaultValue: '50000', description: 'Port number of database instance')
		string(name: 'IOCUSER', defaultValue: 'sysadmin', description: 'IOC user name')
		string(name: 'IOCPASSWORD', defaultValue: 'us3rpa88', description: 'Password of IOC user')
		string(name: 'DBUSER', defaultValue: 'db2i1own', description: 'Database user name')
		string(name: 'DBPASSWORD', defaultValue: 'us3rpa88', description: 'Database user password')
	}
	stages {
		stage('Configure Environment') {
			steps {
				echo "Web Server: ${params.WEBSERVER}"
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

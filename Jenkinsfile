pipeline {
    agent any
    stages {
        stage('Start') {
            options { 
                timestamps() 
                timeout(time: 1, unit: 'HOURS')
            }
            steps {
		sh 'java -jar fitnesse-standalone.jar -d "./" -p 9090 -o -b TestSuite_Results.xml -c "FitNesse.CognitiveCitiesSuiteOfSuites.TestSuites.IocSuite.IocServicesSuite.MiscellaneousIocServices?suite&format=junit"'
            }
            post {
                always {
                    junit 'TestSuite_Results.xml'
                    archiveArtifacts 'TestSuite_Results.xml'
                }
            }
         }
    }
    post {
        always {
		emailext attachmentsPattern: 'TestSuite_Results.xml',
		from: 'admin@jenkins.com',
	        to: 'mooreof@ie.ibm.com',
                subject: "Fitnesse suite complete: ${currentBuild.fullDisplayName}",
                body: "Check attached file for results"
        }
    }
}

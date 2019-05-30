pipeline {
    agent any
    environment { 
        TESTVALUE = 'TRUE'
    }
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
        stage('Start') {
            when {
                environment name: 'TESTVALUE', value: 'TRUE'
            }
            options { 
                timestamps() 
                timeout(time: 5, unit: 'MINUTES')
            }
            steps {
                echo "Hello ${params.PERSON}"
                sh 'printenv'
            }
        }
        stage('Next') {
            when {
                expression { params.PASSWORD ==~ /(SECRET|BLANK)/ }
            }
            steps {
                echo "Hello ${params.PASSWORD}"
                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
                    }
                }                
            }
        }
    }

    post {
        success {
            mail to: 'mooreof@ie.ibm.com',
                subject: "Successful Pipeline: ${currentBuild.fullDisplayName}",
                body: "Hello world"
        }
    }
}

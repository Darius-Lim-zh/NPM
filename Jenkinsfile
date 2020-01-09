pipeline {
    agent any
 
    tools {
        nodejs 'NodeJS'
    }
 
    environment {
        CI = 'true'
    }
 
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh '''
                    set -x
                    npm test
                '''
            }
        }
        stage('Deliver') {
            steps {
                withNPM(npmrcConfig: 'custom-npmrc') {
                    sh '''
                        set -x
                        npm run build
                        set +x
			npm pack

                        npm publish
                    '''
                }
		nexusPolicyEvaluation advancedProperties: '', failBuildOnNetworkError: false, iqApplication: selectedApplication('sandbox-application'), iqScanPatterns: [[scanPattern: '*tgz']], iqStage: 'build', jobCredentialsId: ''
            }
        }
    }
}

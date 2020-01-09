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
                sh '''
		    npm install
		    npm pack
		    nexusPolicyEvaluation advancedProperties: '', failBuildOnNetworkError: false, iqApplication: selectedApplication('sandbox-application'), iqScanPatterns: [[scanPattern: '*tgz']], iqStage: 'build', jobCredentialsId: '' 
		'''
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

                        npm publish
                    '''
                }
            }
        }
    }
}

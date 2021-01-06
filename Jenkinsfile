#!/usr/bin/env groovy

pipeline {

    stages {
        stage('Checkout') {
            steps {
		print "Checkout SCM"
                checkout scm
            }
        }

    stage('Powershell') {
	    when { 
                allOf {
                    // Will not push to repo if the build is unstable at this point.
                    expression { currentBuild.result != 'UNSTABLE' }
                    // Will only publish artifacts from the develop and isolated branch build
                    anyOf { branch 'develop-integration' }
                }
            }
            steps {
		print "Publishing Artifacts to Octopus Deploy"
                withCredentials([string(credentialsId: 'Octo_API_Key', variable: 'OCTOPUS_API_KEY')]) {
                powershell  '''
			C:\\OctopusTools\\octo.exe push `
			--package $ENV:WORKSPACE\\Cerner_Build_Artifacts_Jenkins\\Sitecore_Build_Artifacts_Jenkinsfile.$ENV:BUILD_NUMBER.zip `
			--server https://ctsweboctopus.cerner.com `
			--apiKey $env:OCTOPUS_API_KEY 
		'''
		   
		}
            }
        }
		
	

    post {
        always {
            cleanWs()
        }
    }	    
}

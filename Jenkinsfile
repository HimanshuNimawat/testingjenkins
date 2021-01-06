#!/usr/bin/env groovy

pipeline {
    agent any
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
                    anyOf { branch 'main' }
                }
            }
            steps {
		
                
                powershell  '''
			echo main branch
		'''
		   
		
            }
        }
		
	

    post {
        always {
            cleanWs()
        }
    }	    
}

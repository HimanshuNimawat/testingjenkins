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
                    anyOf { branch 'main' }
                }
            }
            steps {
                powershell  '''
			echo 'main branch'
		'''
            }
        }    
    }
}

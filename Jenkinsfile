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
	    
	stage('Setup') {
            steps {
                script {
                    
                    if (env.BRANCH_NAME == "main") {
                        powershell 'echo \'main\''
                    } else if (env.BRANCH_NAME == "hotfix") {
                        powershell 'echo \'hotfix\''
                    } else if (env.BRANCH_NAME == "release") {
                        powershell 'echo \'release\''
                    }
                }
            }
        }
    }
}

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

    	stage {
    	    steps {
                script {
                     if (env.BRANCH_NAME == main) {
                         powershell '''
			echo 'main branc'
		'''
			  
                       }
                     else if (env.BRANCH_NAME == develop) {
                         powershell '''
			echo 'develop'
		'''
                       }
		     else if (env.BRANCH_NAME == hotfix) {
                         powershell '''
			echo 'hotfix'
		'''
                       }
                   }
             }
         } 
	    
	    post {
		    office365ConnectorSend message: 'Build Success', status: 'Success', webhookUrl: 'https://outlook.office.com/webhook/19c60caa-86b6-4f4c-be9f-32dd22274319@b9d69197-ce1f-4907-9545-b307af2335e9/JenkinsCI/49fa75b94b75428ba8b669754791c526/bf8003f4-23eb-4122-b850-37df323886c1'
	    }
    }
}

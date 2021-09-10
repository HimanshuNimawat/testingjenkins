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

    	stage ('Test') {
    	    steps {
                powershell '''ssh -tt C:\\Users\\HNimawat\\Desktop\\biswa.pem ec2-user@34.207.60.174
			ls
			exit
                '''
            }
        }  
    }
}

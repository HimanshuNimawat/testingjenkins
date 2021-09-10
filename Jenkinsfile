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
                powershell '''ssh -itt C:\\Users\\HNimawat\\Desktop\\biswa.pem ec2-user@34.207.60.174 >>ENDSSH
		ls
                ENDSSH
		'''
            }
        }  
    }
}

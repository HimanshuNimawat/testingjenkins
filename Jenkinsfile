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
                powerhsell '''ssh -i C:\biswa.pem ec2-user@34.207.60.174 "ls"
                '''
            }
        }  
    }
}

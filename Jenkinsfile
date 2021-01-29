#!/usr/bin/env groovy

pipeline {
    agent any
    
	environment{
		MSBUILD_SONAR_HOME = tool 'Sonarqube'
		VERSION_NUMBER = VersionNumber projectStartDate: '', versionNumberString: '${BUILD_DATE_FORMATTED, "yyyy.MM.dd"}.${Build_Number}-dev', versionPrefix: '', worstResultForIncrement: 'SUCCESS'
    }
	
	stages {
	    
		stage('Start Sonarqube Scanner') {
				steps {
					print "Sonarqube Analysis Start"
					withSonarQubeEnv('SonarQube') {
							powershell """
								${env.MSBUILD_SONAR_HOME}\\SonarScanner.MSBuild.exe begin `
									/k:testing `
									/n:testing `
									/v:${env.VERSION_NUMBER} `
									/d:sonar.login=5e5ee5d92b56eb829ad423cc0354f7c72941abc5 `
									/d:sonar.host.url=http://192.168.1.165:9000/ `
							"""
					}
				}
			}
		

		stage('Build') {
				steps {
					print "Building Solution"
					
					powershell '''
							if (Test-Path "C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\BuildTools\\MSBuild\\15.0\\Bin\\MSBuild.exe") {
								Set-Alias msbuild ${env:msBuildPath}\\MSBuild.exe -Scope Script
							} else {
								Write-Error "Cannot find VS 2017 MSBuild"
							}
							msbuild $ENV:WORKSPACE\\CernerComSitecore.sln `
							/p:DeployOnBuild=true `
							/p:Configuration=Release `
							/p:TransformOnBuild=false `
							/p:OutDir=$ENV:WORKSPACE\\Cerner_Build_Artifacts_Jenkins `
							/p:DeleteExistingFiles=false `
							/p:BuildProjectReferences=true `
							/p:AllowedReferenceRelatedFileExtensions=.pdb
						'''
					
				}
			}

		stage('SonarQube Analysis') {
				steps {
					withSonarQubeEnv('SonarQube') {
						withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
							powershell """
								${env.MSBUILD_SONAR_HOME}\\SonarScanner.MSBuild.exe end `
								/d:sonar.login=5e5ee5d92b56eb829ad423cc0354f7c72941abc5 
							"""
						}
					}
				}
			}

		stage('SonarQube-QualityGate') {
				steps {
						timeout(time: 1, unit: 'HOURS') {
							waitForQualityGate abortPipeline: true
					}
				}
			}
}


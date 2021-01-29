#!/usr/bin/env groovy

pipeline {
    agent any
    
	environment{
		
		VERSION_NUMBER = VersionNumber projectStartDate: '', versionNumberString: '${BUILD_DATE_FORMATTED, "yyyy.MM.dd"}.${Build_Number}-dev', versionPrefix: '', worstResultForIncrement: 'SUCCESS'
    }
	
	stages {
	    
		stage('Start Sonarqube Scanner') {
				steps {
					print "Sonarqube Analysis Start"
					withSonarQubeEnv('Sonarqube') {
							powershell """
							    cd "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\MSBuild\\Current\\Bin"
								C:\\Jenkins\\sonar-scanner\\SonarScanner.MSBuild.exe begin `
									/k:testing `
									/n:testing `
									/v:${env.VERSION_NUMBER} `
									/d:sonar.login=5e5ee5d92b56eb829ad423cc0354f7c72941abc5 `
									/d:sonar.host.url=http://192.168.1.165:9000/ `
							"""
					}
				}
			}
		
		stage('Nuget_Restore') {
			steps {
			print "Restoring Nuget Packages on sln"
			powershell '''
			    cd "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\MSBuild\\Current\\Bin"
				C:\\nuget\\nuget.exe restore C:\\Sonarqube_btlaw-test\\BTLaw.sln -source "https://api.nuget.org/v3/index.json" `
				-source "https://sitecore.myget.org/F/sc-packages/api/v3/index.json" -source "https://teamcity.mkcsites.com/httpAuth/app/nuget/feed/_Root/default/v2/"
				'''
			}
		}
		

		stage('Build') {
				steps {
					print "Building Solution"
					
					powershell '''
							
							cd "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\MSBuild\\Current\\Bin"
							./MSBuild.exe C:\\Sonarqube_btlaw-test\\BTLaw.sln `
							/p:DeployOnBuild=true  ` 
							/p:Configuration=release `
							/p:DeployDefaultTarget=WebPublish `
							/p:WebPublishMethod=FileSystem `
							/p:DeleteExistingFiles=false `
							/p:BuildProjectReferences=true `
							/p:publishUrl=C:\\Denton_Artifact_Sonarqube\\
						'''
					
				}
			}

		stage('SonarQube Analysis') {
				steps {
					withSonarQubeEnv('Sonarqube') {
							powershell """
							    cd "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\MSBuild\\Current\\Bin"
								C:\\Jenkins\\sonar-scanner\\SonarScanner.MSBuild.exe end `
								/d:sonar.login=5e5ee5d92b56eb829ad423cc0354f7c72941abc5 
							"""
						}
					}
				}
			

		stage('SonarQube-QualityGate') {
				steps {
					powershell """
					  cd "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\MSBuild\\Current\\Bin"
					  """
						timeout(time: 1, unit: 'HOURS') {
							waitForQualityGate abortPipeline: true
					}
				}
			}
			
		}
	}




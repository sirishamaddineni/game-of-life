pipeline {
	agent any
	options {
		 disableConcurrentBuilds()
           }
	
	 stages{
		 stage ( 'clone' ){
		    steps{
			git "https://github.com/sirishamaddineni/game-of-life"
		    }
		}
		stage ( 'complie' ){
		    steps{
		       withMaven( maven : 'Maven 3.5.2' ){
		       bat 'mvn clean compile'
		    }
		   }
		}
		stage ('testing' ){
		    steps{
		        withMaven( maven : 'Maven 3.5.2' ){
		        bat 'mvn test'
		       }
		    }
		}
		stage( 'IQ_Scan' ){
		     steps{
			nexusPolicyEvaluation failBuildOnNetworkError: false, 
				iqApplication: 'IQ_app', 
				iqScanPatterns: [[scanPattern:  '**/gameoflife-core.jar']], 
				iqStage: 'release', 
				jobCredentialsId: 'NexusIQCred'
			     
     			}
     		}
		stage ( "Tagging" ){                	  
 			steps {
                         bat "git tag 'v31.1'"
                	 bat "git config user.email 'sirishamaddineni25@gmail.com'"
                         bat "git config user.name 'sirishamaddineni'"	
			}
		}
			 
		stage( "Deploy" ){
		      steps{
				nexusArtifactUploader artifacts: [[artifactId: 'gameoflife-core', classifier: '', file: 'gameoflife-core/build/libs/gameoflife-core.jar', type: 'jar']],
				credentialsId: '5aed14d0-99ce-4bd4-bbad-b6e3014c4e28', 
				groupId: 'maven-public', 
				nexusUrl: 'localhost:9091', 
				nexusVersion: 'nexus3', 
				protocol: 'http', 
				repository: 'GameOfLife', 
				version: '3.0'
			}
		}
    	   }
     }


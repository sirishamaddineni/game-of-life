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
				iqApplication: 'GOL_App',
				iqScanPatterns: [[scanPattern: '**/*.jar' ]],
				iqStage: 'release',
				jobCredentialsId: ''
                     }
     		}
					 
		stage( "Deploy to Nexus" ){
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
		 stage( "credentials"){
			 steps{
		withCredentials([usernamePassword(credentialsId: 'sirishamaddineni', usernameVariable: 'Sirisha', passwordVariable: '9553902999')]) 
	        {
	             bat 'echo $PASSWORD'
                     echo USERNAME
                     echo "username is $USERNAME"
		}
            }
	   }
	 }
     }


#!groovy

import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException

env.isMasterBranch = false
echo "The parameter isMasterBranch default value is: ${env.isMasterBranch}"

pipeline {

	agent none

	options {
		buildDiscarder logRotator( daysToKeepStr: '8', numToKeepStr: '5') 
	}

	environment {
		MyCustomParam1 = "myCustomValue"
	}


  	stages {

	stage('Setting parameters') {
		when {
			branch "master"
		}
    	steps {
    		script {
          		node {
        				env.isMasterBranch = true
        				println "\nThe parameter isMasterBranch value has changed.\nTheir new value is: ${env.isMasterBranch}\n"
          		}
        	}
    	}
	} // Stage Setting parameters closed.

	stage("Unit Tests") {
		steps {
			script {
				node {
					highlightStage("Unit Tests")
					sh '''
						echo "TO DO - Add unit tests execution."
					'''
				}
			}
		}
	}

	stage("Deploy") {
		steps {
			script {

					highlightStage("Deploy")

					if(env.isMasterBranch.toBoolean()) {

						def abortFlag = false;
						def didTimeout = false;

						try {
							input(message: "Are you sure that you want to Deploy to the Production environment ?")
							
						}
						catch(FlowInterruptedException abortError){
							
								echo "The deployment to production was Aborted automatically by the default timeout. (${user})"
						    
						}
						catch(Exception exc) {
					        echo "echo 'Deploy stage error: ${exc.toString()}'"
						}
						finally {
							
						}
					}
					else {
						//deployToEnvironment("Development", DEV_AWS_ACCESS_KEY, DEV_AWS_SECRET_KEY, AWS_REGION, AWS_SERVICE_NAME, STASH_NAME);
					}
						
			}
		}
	}

  } // stages closed

}

def highlightStage(stageName) {
	ansiColor('xterm') {
		echo "\033[42m  +++++++++++++++++++ ${stageName} Stage +++++++++++++++++++ \033[0m"
	}
}

def removeAllFiles() {
	sh "rm -rf **/*"
	sh "rm -rf *"
}

def deployToEnvironment(environmentName, envAwsAccessKey, envAwsSecretKey, region, serviceName, stashName ){

	stage("AWS-Deploy to ${environmentName}.") {
		script {
			node {
				//docker.withRegistry("http://incontact-docker-snapshot-local.jfrog.io","${JFROG_ARTIFACTORY_BUILD_CREDENTIAL}") {
              		//docker.image("cicd-awscli-toolbox2:latest").inside("-u root:root"){

						highlightStage("AWS-Deploy to ${environmentName}.")

						def secretsList = []
						//secretsList.add(string(credentialsId: "${envAwsAccessKey}", variable: "AWS_ACCESS_KEY"))
						//secretsList.add(string(credentialsId: "${envAwsSecretKey}", variable: "AWS_SECRET_KEY"))

						def environmentParamsList = []
					  //environmentParamsList.add( "AWS_REGION=${region}" )
						//environmentParamsList.add( "SERVICE_NAME=${serviceName}" )
						//environmentParamsList.add( "CLUSTER_NAME=${serviceName}-stack" )

						//withCredentials( secretsList ) {
							//withEnv( environmentParamsList ) {

								removeAllFiles()

								unstash "$stashName"
								myParam1 = "${AWS_ACCESS_KEY}"

								dir("$stashName"){
									sh '''
										echo "Hola mundo"
									'''	
								}

							//} // withEnv
						//} // withCredentials

					  //}
				//}
			}
		}
	} // Stage AWS-Deploy to xxx Closed.
}

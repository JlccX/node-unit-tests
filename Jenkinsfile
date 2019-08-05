#!groovy

import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException

env.isMasterBranch = false
echo "The parameter isMasterBranch default value is: ${env.isMasterBranch}"

pipeline {

	agent none

	environment {
		MyCustomParam1 = "myCustomValue"
		stashName = "customFolder" + ${BUILD_NUMBER}
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

					echo '''
						TO DO - Add unit tests execution.
					'''
					dir(stashName) {
						checkout scm
						sh "dir"
					}
					stash name: "${stashName}", include: "${stashName}/**"
				}
			}
		}
	}

	stage("Deploy") {
		steps {
			script {

					highlightStage("Deploy")
					unstash "${stashName}"

					if(env.isMasterBranch.toBoolean()) {

						def abortFlag = false;
						def didTimeout = false;

						try {
							timeout(time: 60, unit: "SECONDS"){
								input(message: "Are you sure that you want to Deploy to the Production environment ?")
							}
						}
						catch(FlowInterruptedException abortError){
							def user = abortError.getCauses()[0].getUser()
							
							// SYSTEM means timeout.
							if('SYSTEM' == user.toString()) { 
						        didTimeout = true
								echo "The deployment to production was Aborted automatically by the default timeout. (${user})"
						    }
							// If user is distinct from SYSTEM, then a jenkins user aborted the pipeline deploy.
							else {
								println "The Deploying to production process was canceled by the user: ${user}"
								abortFlag = true
							}
							
						}
						catch(Exception exc) {
					        echo "echo 'Deploy stage error: ${exc.toString()}'"
						}
						finally {
							if(didTimeout) {
								sh "echo 'The deploy to production environment was automatically canceled because the user did not confirmed the deploy before to the timeout.'"
							}
							if(didTimeout && abortFlag) {
								
								deployToEnvironment("Production",stashName);
								
							}
						}
					}
					else {
						deployToEnvironment("Development",stashName);
					}
						
			}
		}
	}

  } // stages closed

}

def highlightStage(stageName) {
	//ansiColor('xterm') {
		println "\033[42m  +++++++++++++++++++ ${stageName} Stage +++++++++++++++++++ \033[0m"
	//}
}

def removeAllFiles() {
	sh "rm -rf **/*"
	sh "rm -rf *"
}

def deployToEnvironment(environmentName, stashName){

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

								//removeAllFiles()

								unstash "$stashName"
								myParam1 = "${AWS_ACCESS_KEY}"

								dir("$stashName"){
									emailext body: 'hello, the jenkins pipeline was executed.', subject: 'jenkins test', to: 'jlccx@live.com'
									//sh '''
									//	echo "Hola mundo"
									//'''	
								}

							//} // withEnv
						//} // withCredentials

					  //}
				//}
			}
		}
	} // Stage AWS-Deploy to xxx Closed.
}

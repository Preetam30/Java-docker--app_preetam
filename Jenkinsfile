pipeline {
	agent {	
		label 'pipeline-1'
		}
	stages {
		stage("SCM") {
			steps {
				git branch: 'main', url: 'https://github.com/Preetam30/Java-docker--app_preetam.git'
				}
			}

		stage("Build") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'
				sh 'sudo mvn clean package'
				}
			}
		stage("Image") {
			steps {
				sh 'sudo docker build -t java-repo . '
				sh 'sudo docker tag java-repo preetam30/pipeline-java'
				}
			}
				
	
		stage("Docker_Hub") {
			steps {
			withCredentials([string(credentialsId: 'docker_hub_preetam', variable: 'docker_hub_password_var')]) {
				sh 'sudo docker tag java-repo preetam30/pipeline-java'
				sh 'sudo docker push preetam30/pipeline-java'
				}
			}	

		}
		stage("QAT Testing") {
			steps { 
				sh 'sudo docker run -d nginx'
				sh 'sudo docker rm -f $(sudo docker ps -a -q)'
				sh 'sudo docker run -dit -p 8080:8080 --name web11 preetam30/pipeline-java'
			}
		}
		stage('QAT testing') {
			steps {
				script{
			 		retry(4) {
			    		test='${curl --silent http://65.0.32.12:8080/java-web-app/ && grep -iE "mahi|devops"}'
						echo "${test}"
			 		}
				}    
			}
		}

		stage("Approval status") {
			steps {
				script {  
               	 Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
               	 echo 'userInput: ' + userInput
				}
	 		}
		}
	}
}



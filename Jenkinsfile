pipeline {
	agent {	
		label 'Slave_1'
		}
	stages {
		stage("SCM") {
			steps {
				git 'https://github.com/Mahigurjarr/java-docker-app_ronak.git'
				}
			}

		stage("build") {
			steps {
				sh 'sudo mvn dependency:purge-local-repository'
				sh 'sudo mvn clean package'
				}
			}
		stage("Image") {
			steps {
				sh 'sudo docker build -t java-repo:$BUILD_TAG .'
				sh 'sudo docker tag java-repo:$BUILD_TAG mahigurjarr/pipeline-java:$BUILD_TAG'
				}
			}
				
	
		stage("Docker Hub") {
			steps {
			withCredentials([string(credentialsId: 'Docker_hub_mahi', variable: 'docker_hub_password_var')]) {
				sh 'sudo docker login -u mahigurjarr -p ${docker_hub_password_var}'
				sh 'sudo docker push mahigurjarr/pipeline-java:$BUILD_TAG'
				}
			}	

		}
		stage("QAT Testing") {
			steps {
				sh 'sudo docker run -dit -p 8080:8080 --name web11 mahigurjarr/pipeline-java:$BUILD_TAG'
				}
			}
	}
}


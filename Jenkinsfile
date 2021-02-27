pipeline {
	agent any 
	environment {
		DOCKER_HUB_REPO = "vakkasoglu/capstone-project"
		CONTAINER_NAME = "capstone-project"
		REGISTRY_CREDENTIAL = "dockerhub"

	}
	stages {
		stage('Clean up') {
			steps {
				script {
					sh 'rm -rf jenkins-docker-image-build'
				}
			}
		}
		stage('Clone repository') {
			steps {
				script {
					sh 'git clone https://github.com/mvakkasoglu/jenkins-docker-image-build.git'
				}
			}
		}
		stage('Build docker image') {
			steps {
				script {
					dir('./jenkins-docker-image-build') {
						sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
						sh 'docker image tag $DOCKER_HUB_REPO:latest $DOCKER_HUB_REPO:$BUILD_NUMBER'
					} 
				}
			}
		}
		stage('Push docker image') {
		    steps {
			      script {
				        dir('./jenkins-docker-image-build') {
				            docker.withRegistry( '', REGISTRY_CREDENTIAL ) {
				                sh 'docker push vakkasoglu/capstone-project:$BUILD_NUMBER'
				                sh 'docker push vakkasoglu/capstone-project:latest'
			                }
					      }
				    }
			  }
		}
		// stage('Deploy to kubernetes') {
		// 	steps {
		// 		script {
		// 			sh 'kubectl apply -f kubernetes.yaml'
        //             sh 'kubectl get service/capstone-project-service'
        //             sh 'minikube service capstone-project-service'
		// 		}
		// 	}
		// }
	}
}
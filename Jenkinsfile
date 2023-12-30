pipeline {

    options{
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
    }

    agent any

    tools{
        maven 'maven_3.9.4'
    }

	stages {
		stage('Code Compilation') {
			steps {
				echo 'Code Compilation is in progress!'
				sh 'mvn clean compile'
				echo 'Code Compilation is Completed Successfully!'
			}

		}
		stage('Code QA Execution') {
			steps {
				echo 'Junit Test case check in Progress!'
				sh 'mvn clean test'
				echo 'Junit Test case check Completed!'
			}

		}
		stage('Code Package') {
			steps {
				echo 'Creating war Artifact'
				sh 'mvn clean package'
				echo 'Creating war artifact done'
			}
		}
		stage('Building & Tag Docker Image') {
        			steps {
        				echo 'Starting Building Docker Image'
        				sh 'docker build -t yaashu2107/makemytrip .'
        				sh 'docker build -t makemytrip .'
        				echo 'Completed Building Docker Image'
        			}
        		}
        stage('Docker Image Scanning') {
        			steps {
        				echo 'Docker Image Scanning Started'
        				sh 'java --version'
        				echo 'Docker Image Scanning Started'
        			}
        		}
        stage('Docker push to Docker Hub') {
        			steps {
        			    script {
        			        withCredentials([string(credentialsId: 'dockerhubCred', variable:'dockerhubCred')]){
        				    sh 'docker login docker.io -u yaashu2107 -p ${dockerhubCred}'
        				    echo "Push Docker Image to Docker Hub: In Progress"
        				    sh 'docker push yaashu2107/makemytrip:latest'
        				    echo "Push Docker Image to Docker Hub: In Progress"
        				    sh 'whoami'
        				    }
        			    }
        			}
        		}
        stage('Docker Image push to Amazon ECR') {
                	steps {
                		script {
                	        withDockerRegistry([credentialsId: 'ecr:ap-south-1:ecr-credentials', url:"https://559220132560.dkr.ecr.ap-south-1.amazonaws.com"]){
                			sh """
                			echo "List the docker images present in local"
                			docker images
                			echo "Tagging the Docker Image: In Progress"
                			docker tag makemytrip:latest 559220132560.dkr.ecr.ap-south-1.amazonaws.com/makemytrip:latest
                			echo "Tagging the Docker Image: Completed"
                			echo "Push Docker Image to ECR: In Progress"
                			docker push 559220132560.dkr.ecr.ap-south-1.amazonaws.com/makemytrip:latest
                			echo "Push Docker Image to ECR: Completed"
                			"""
                			}
                		}
                	}
        }
	}
}
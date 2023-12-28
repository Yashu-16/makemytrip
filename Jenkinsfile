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
	}
}
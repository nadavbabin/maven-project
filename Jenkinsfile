pipeline {
	agent any
	stages{
		stage('Init'){
			steps {
				echo "Testing.."
			}
		}

		stage('Build'){
			steps{
				sh 'mvn clean package'
			}
		}

		stage('Deploy'){
			steps{
				echo 'Now Archiving'
				archiveArtifacts artifacts : '**/target/*.war'
			}
		}
	}
}
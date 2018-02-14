pipeline {
    agent any
    tools {
    	maven 'localMaven'
    }


    def myFunc(){
    	echo '************************'
    	echo '***** my function ******'
    	echo '************************'
    }


    stages{
        stage('Build'){
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('call-to-my-func'){
        	myFunc();
        }

        stage('Deploy to Staging'){
        	steps{
        		echo 'Starting to Deploy'
        		build job: 'deploy-to-staging'
        	}
        }
    }
}
import jenkins.model.*
jenkins = Jenkins.instance

def myFunc(){
    	echo '************************'
    	echo '***** my function ******'
    	echo '************************'
    }

pipeline {
    agent any
    tools {
    	maven 'localMaven'
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
        	steps {
        		script {
        			 echo '***************************'
        			 echo '******** STARTED SCRIPT *******'
        			 echo '***************************'
        			 def name = true
        			 if(name){
        			 	echo 'the value is true'
        			 }
        			 else{
        			    echo 'the value is false'
        			 }
        		}
        	}
        }

        stage('Checkout') {
            steps {
            	script {
					 	git 'https://github.com/jenkinsci/last-changes-plugin.git'
                		lastChanges since: 'LAST_SUCCESSFUL_BUILD', format:'SIDE',matching: 'LINE'            	
            	}

            }
        }

        stage('Deploy to Staging'){
        	steps{
        		echo 'Starting to Deploy'
        		build job: 'deploy-to-staging'
        	}
        }
    }
}
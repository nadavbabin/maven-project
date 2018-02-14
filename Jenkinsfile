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
        		}
        	}
        }

      stage("checkout") {
      	  steps {
	        git url: 'https://github.com/nadavbabin/maven-project.git'
      	  }
      }

      stage("last-changes") {
      	steps {
      		script {
      		  def publisher = LastChanges.getLastChangesPublisher "PREVIOUS_REVISION", "SIDE", "LINE", true, true, "", "", "", "", ""
              echo '***************************'
              echo '******* Publisher ************'
              println(publisher)
              echo '***************************'
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
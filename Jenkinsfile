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

      

      stage("last-changes") {
      	steps {
      		script {
              echo '**************************************'
              echo '***********  SECOND TRY  *************'
              	  def branch = sh(returnStdout : true , script : "git branch").trim()
              	  def diff = sh(returnStdout : true , script : "git show --name-only").trim()
    	          println(branch)
    	          echo '-__________________________-------'
    	          println(diff)
              echo '**************************************'
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
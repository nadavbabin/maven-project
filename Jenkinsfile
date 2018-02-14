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


      stage("checkout") {
      		steps {
      			script {
			        git url: 'https://github.com/jenkinsci/last-changes-plugin.git'
      			}
      		}
      }

      stage("last-changes") {
      	steps
      		{
      			script {
		        	      def publisher = LastChanges.getLastChangesPublisher "PREVIOUS_REVISION", "SIDE", "LINE", true, true, "", "", "", "", ""
			              publisher.publishLastChanges()
			              def changes = publisher.getLastChanges()
			              println(changes.getEscapedDiff())
			              for (commit in changes.getCommits()) {
			                  println(commit)
			                  def commitInfo = commit.getCommitInfo()
			                  println(commitInfo)
			                  println(commitInfo.getCommitMessage())
			                  println(commit.getChanges())
			              }      			
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
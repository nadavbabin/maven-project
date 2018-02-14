

def myFunc(){
    	echo '************************'
    	echo '***** my function ******'
    	echo '************************'
    }


def commitHashForBuild(build) {
	def scmAction = build?.actions.find { action -> action instanceof jenkins.scm.api.SCMRevisionAction }
	return scmAction?.revision?.hash
}


def getLastSuccessfulCommit() {
  def lastSuccessfulHash = null
  def lastSuccessfulBuild = currentBuild.rawBuild.getPreviousSuccessfulBuild()
  if ( lastSuccessfulBuild ) {
    lastSuccessfulHash = commitHashForBuild(lastSuccessfulBuild)
  }
  return lastSuccessfulHash
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
        			  def lastSuccessfulCommit = getLastSuccessfulCommit()
					  def currentCommit = commitHashForBuild(currentBuild.rawBuild)
					  if (lastSuccessfulCommit) {
					    commits = sh(
					      script: "git rev-list $currentCommit \"^$lastSuccessfulCommit\"",
					      returnStdout: true
					    ).split('\n')
					    echo '********************************************'
					    println "Commits are: $commits"
					    echo '********************************************'
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
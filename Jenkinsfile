#!groovy

pipeline {
    agent {
    	label "docker-build"
    }
	
    options {
        timestamps()
        skipStagesAfterUnstable()
    }

    stages {
    	stage('Build Docker image') {
            steps {
		sh '''#!/usr/bin/env bash
		echo "Starting build for Eclipse Codewind Che plugin..."
		./scripts/build.sh
		'''
            }
        }
        
        stage('Publish Docker image') {
            steps {
                withDockerRegistry([url: 'https://index.docker.io/v1/', credentialsId: 'docker.com-bot']) {
		    sh '''#!/usr/bin/env bash
			if [ -z $CHANGE_ID ]; then
			    echo "Publishing docker images for Eclipse Codewind Che Sidecar..."
			    ./scripts/publish.sh eclipse
			else
		    	    echo "Skip publishing docker images for the PR build"
			fi
		    '''
	    	}
	    }
        }
    }
}

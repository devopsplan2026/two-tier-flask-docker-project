		pipeline {

            agent any

	    //////agent { label 'dev' }

	    environment {
		DOCKER_HUB = "devoopsguru"
		IMAGE_NAME = "two-tier-flask-app"

		IMAGE_LATEST = "${DOCKER_HUB}/${IMAGE_NAME}:latest"
		IMAGE_BUILD  = "${DOCKER_HUB}/${IMAGE_NAME}:${BUILD_NUMBER}"
	    }

	    stages {

		stage('Code Clone') {
		    steps {
		        git branch: 'main',
		            url: 'https://github.com/devopsplan2026/two-tier-flask-docker-project.git'
		    }
		}

		stage('Build Docker Image') {
		    steps {
		        sh """
		            docker build -t ${IMAGE_LATEST} .
		            docker tag ${IMAGE_LATEST} ${IMAGE_BUILD}
		        """
		    }
		}

		stage('Test') {
		    steps {
		        echo "Application testing will be added here."
		    }
		}

		stage('Docker Login') {
		    steps {
		        withCredentials([
		            usernamePassword(
		                credentialsId: 'docker_cred',
		                usernameVariable: 'DOCKER_USERNAME',
		                passwordVariable: 'DOCKER_PASSWORD'
		            )
		        ]) {
		            sh '''
		            echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
		            '''
		        }
		    }
		}

		stage('Push Docker Images') {
		    steps {
		        sh """
		            docker push ${IMAGE_LATEST}
		            docker push ${IMAGE_BUILD}
		        """
		    }
		}

		stage('Remove Local Images') {
		    steps {
		        sh """
		            docker image rm ${IMAGE_LATEST} || true
		            docker image rm ${IMAGE_BUILD} || true
		        """
		    }
		}
	    }

	    post {

		success {
		    emailext(
		        from: 'vishalshamkaria@gmail.com',
		        to: 'vishalshamkaria@gmail.com',
		        subject: 'Jenkins Build Success',
		        body: """
	Build completed successfully.

	Docker Images:
	${IMAGE_LATEST}
	${IMAGE_BUILD}

	Both images have been pushed to Docker Hub.
	"""
		    )
		}

		failure {
		    emailext(
		        from: 'vishalshamkaria@gmail.com',
		        to: 'vishalshamkaria@gmail.com',
		        subject: 'Jenkins Build Failed',
		        body: """
	Build failed.

	Please check the Jenkins console output for details.
	"""
		    )
		}

		always {
		    cleanWs()
		}
	    }
	}

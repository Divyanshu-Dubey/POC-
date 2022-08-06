node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build ("your-git-id/your-git-repo")
        
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }
	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-dubey')
	}


   stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}



    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
     //  docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
        
           
               
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        
    }
    post {
		always {
			sh 'docker logout'
		}
	}
}

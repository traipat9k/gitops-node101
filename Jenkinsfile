pipeline {
	agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "nodehello"
			  /*IMAGE_TAG = "1.0.0-6"*/
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        
		
        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'traipat9kt', url: 'https://github.com/traipat9k/gitops-node101'
               }
        }
     
		
        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "traipat9k"
                   git config --global user.email "traipatk@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """

				withCredentials([gitUsernamePassword(credentialsId: 'traipat9kt', gitToolName: 'Default')]) {
                  sh "git push https://github.com/traipat9k/gitops-node101 main"
                }
				
            }
        }
      
    }
}
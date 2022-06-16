
pipeline{

	agent any

	
	stages {

		stage('Build') {

			steps {
				sh 'sudo docker build -t rohini3/aws-dkr:eks .'
               
			}
		}
        
        stage('login') {

            steps {
        
		        withCredentials([string(credentialsId: 'DOCKER_PWD', variable: 'PASSWORD')]) {
                    sh 'sudo docker login -u rohini3 -p Myhub123!'
                }
            }
        }
		stage('Push') {

			steps {
				sh 'sudo docker push rohini3/aws-dkr:eks'
			}
		}

        stage('eks deploy') {

			steps {
				sh 'kubectl get -o yaml deploy/demo-nodejs.yml > deploy.yaml'
                sh "sed -i 's/rohini3:latest/rohini3:eks/g' deploy.yaml"
                sh 'kubectl apply -f deploy.yaml'
                sh 'kubectl rollout restart deployment demo-nodejs.yml'
			}
		}
	}

	post {
		always {

            		cleanWs()
			
            		echo "done"
		}
	}

}


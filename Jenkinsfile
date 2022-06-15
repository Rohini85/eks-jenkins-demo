
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
                    sh 'sudo docker login -u Rohini3 -p $PASSWORD'
                }
            }
        }
		stage('Push') {

			steps {
				sh 'sudo docker push Rohini3/aws-dkr'
			}
		}

        stage('eks deploy') {

			steps {
				sh 'kubectl get -o yaml deploy/hello-world-nodejs > deploy.yaml'
                sh "sed -i 's/hellonodejs:latest/hellonodejs:eks/g' deploy.yaml"
                sh 'kubectl apply -f deploy.yaml'
                sh 'kubectl rollout restart deployment hello-world-nodejs'
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


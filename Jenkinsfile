pipeline {

	agent any


	stages {

		stage('Build') {

			steps {
				sh 'sudo docker build -t rohini3/eks-repo:eks .'

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
				sh 'sudo docker push rohini3/eks-repo:eks'
			}
		}

		stage('eks deploy') {

			steps {
				sh 'kubectl get deploy/hello-world-nodejs > deploy.yaml'
				sh "sed -i 's/eks-repo:latest/eks-repo:eks/g' deploy.yaml"
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

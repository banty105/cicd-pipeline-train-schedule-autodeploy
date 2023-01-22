pipeline {
    environment {
	dockerimagename = "satya105/myimages:bestimage"    
    }
    agent any
    stages {
        
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/banty105/cicd-pipeline-train-schedule-autodeploy.git'
            }
        }
/*	    
	stage('Gradle Build') {
            steps {
                sh "./gradlew build"
            }
        }
*/
        stage('Docker Image') {
            steps {
		sh 'docker build -t ${dockerimagename} . '
            }
        }
	        
	stage('Push to Docker Hub') {
                steps {
			script {
				withCredentials([string(credentialsId: 'DOCKER_LOGIN', variable: 'xyz')]) {
                                     sh "docker login -u satya105 -p ${xyz}"
		                     sh "docker push ${dockerimagename}"
				}		
		}	
            }
        }
		
	stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f train-schedule-kube.yml --kubeconfig /tmp/admin.conf"
            }
        }	
    }
}

pipeline {
    environment {
	dockerimagename = "satya105/myimages:bestimage"    
    }
    agent none
    stages {
        
        stage('SCM Checkout') {
            agent { label 'slaveNode'}
            steps {
                git 'https://github.com/banty105/cicd-pipeline-train-schedule-autodeploy.git'
            }
        }

        stage('Docker Image') {
            agent { label 'slaveNode'}
            steps {
		sh 'docker build -t ${dockerimagename} . '
            }
        }
	    
	stage('Push to Docker Hub') {
            agent { label 'slaveNode'}
            steps {
                sh "docker login -u satya105 -p ${DOCKER_LOGIN}"
		sh "docker push ${dockerimagename}"
            }
        }
		
	stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f train-schedule-kube.yml"
            }
        }	
    }
}

pipeline {
    agent any
    environment {
        IMAGE_NAME = "hemanathan18/employee-app"
    }
    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/Hemanathan-N/project-K8s.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                    sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }
        stage('Push Image to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                credentialsId: 'DockerHub', 
                passwordVariable: 'docker_pwd', 
                usernameVariable: 'docker_un')])  {
                        sh '''
                        docker login -u ${docker_un} -p ${docker_pwd}
                        docker push $IMAGE_NAME:latest  
                        ''' 
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                    sh '''
                    kubectl get nodes
                    kubectl apply -f k8s/deployment.yml
                    kubectl apply -f k8s/service.yml
                    kubectl get deployments
                    kubectl get pods
                    kubectl get svc
                    ''' 
            }
        }
    }
    post {
        success {
            emailext(
                subject: "Jenkins Build Successful !",
                body: "Jenkins K8s-Deploy-App-pipeline completed successfully.",
                to: "hemanathan18180@gmail.com" 
            ) 
        }
        failure {
            emailext(
                subject: "Jenkins Build Failed !!",
                body: "Jenkins K8s-Deploy-App-pipeline failed. Please check logs.",
                to: "hemanathan18180@gmail.com"
            )
        }
    }
}

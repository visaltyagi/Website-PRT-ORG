pipeline {
    agent none
    environment {
        DOCKERHUB_CREDENTIALS=credentials('8265e363-f326-4be9-a3a6-89563b60c4a4')
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Docker') {
            agent {
                label 'Kubernetes-Agent'
            }
            steps {
                sh 'sudo docker build /home/ubuntu/jenkins/workspace/PRTPipeline -t visaltyagi12/prttest'
                sh 'sudo echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'sudo docker push visaltyagi12/prttest'
            }
        }
        stage('K8s') {
            agent {
                label 'Kubernetes-Agent'
            }
            steps {
                sh 'kubectl delete deploy nginx-deployment'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}

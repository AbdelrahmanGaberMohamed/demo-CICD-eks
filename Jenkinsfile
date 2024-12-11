pipeline {
    agent any

    environment {
        IMAGE_NAME = 'abdelrahmangaber/jenkins-test'
        IMAGE_TAG = "${IMAGE_NAME}:${env.GIT_COMMIT}"
        //KUBECONFIG = credentials('kubeconfig-credentials-id')
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
        CLUSTER_NAME = 'demo-eks'   
    }

    
    stages {
        stage('Setup') {
            steps {
                sh '''
                    ls -la $KUBECONFIG
                    chmod 644 $KUBECONFIG
                    ls -la $KUBECONFIG'
                    python3 -m venv app
                    app/bin/pip install -r requirements.txt
                '''

            }
        }
        stage('Test') {
            steps {
                sh "app/bin/pytest"
            }
        }

        stage('Login to docker hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh 'echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin'}
                echo 'Login successfully'
            }
        }

        stage('Build Docker Image')
        {
            steps
            {
                sh 'docker build -t ${IMAGE_TAG} .'
                echo "Docker image build successfully"
                sh 'docker image ls'
                
            }
        }

        stage('Push Docker Image')
        {
            steps
            {
                sh 'docker push ${IMAGE_TAG}'
                echo "Docker image push successfully"
            }
        }

        stage('Deploy to EKS')
        {
            steps {
                sh '''
                    aws eks update-kubeconfig --name ${CLUSTER_NAME}
                    kubectl set image deployment/flask-app flask-app=${IMAGE_TAG}
                '''
            }
        }         
    }
}
pipeline {
    agent none

    environment {
        DOCKERHUB_CREDENTIALS = credentials('24a15a19-e7b4-417d-8117-55a38345e2a8')
    }

    stages {

        stage('Hello') {
            agent { label 'master' }

            steps {
                echo 'Hello World'
            }
        }

        stage('Git') {
            agent { label 'master' }

            steps {
                git 'https://github.com/mrred009/cap1'
            }
        }

        stage('Docker') {
            agent { label 'master' }

            steps {

                sh 'sudo docker build -t mrred008/project2 .'

                sh '''
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | \
                    sudo docker login \
                    -u "$DOCKERHUB_CREDENTIALS_USR" \
                    --password-stdin
                '''

                sh 'sudo docker push mrred008/project2'
            }
        }

        stage('Kubernetes') {
            agent { label 'master' }

            steps {

                sh '''
                    sudo KUBECONFIG=/etc/kubernetes/admin.conf kubectl apply \
                    -f /home/ubuntu/jenkins/workspace/job1/deploy.yaml
                '''

                sh '''
                    sudo KUBECONFIG=/etc/kubernetes/admin.conf kubectl apply \
                    -f /home/ubuntu/jenkins/workspace/job1/svc.yaml
                '''
            }
        }
    }
}

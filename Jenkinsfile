pipeline {
    agent any
    environment {
        NEXUS_CREDS = credentials('nexus_creds')
        NEXUS_DOCKER_REPO = '100.102.123.78:8081/repository/docker-images-repo-private/'
    }

    stages {
       
       stage('Docker Build') {
        
            steps { 
                    echo 'Building docker Image'
                    sh 'docker build -t $NEXUS_DOCKER_REPO/go-1:$BUILD_NUMBER .'
                }
        }

       stage('Docker Login') {
            steps {
                echo 'Nexus Docker Repository Login'
                script{
                    withCredentials([usernamePassword(credentialsId: 'nexus_creds', usernameVariable: 'admin', passwordVariable: 'admin123' )]){
                       sh ' echo $PASS | docker login -u $USER --password-stdin $NEXUS_DOCKER_REPO'
                    }
                   
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing Imgaet to docker hub'
                sh 'docker push $NEXUS_DOCKER_REPO/fakeweb:$BUILD_NUMBER'
            }
        }
    }
}
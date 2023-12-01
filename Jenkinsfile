pipeline {
    agent any
    environment {
        PROJECT_ID = 'rancher'
        CLUSTER_NAME = 'rancher'
        LOCATION = 'us-east-1a'
        DOCKERHUB_PASS = 'AJ@gmu-2024'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage('BuildJAR') {
            steps {
                echo 'Building the JAR ...'
                sh 'java -version'
                sh 'cp target/survey-0.0.1-SNAPSHOT.jar .'  // Assuming Maven is used to build the JAR file
                sh 'docker login -u ajagadis -p ${DOCKERHUB_PASS}'
                sh 'docker build -t ajagadis/645 .'
            }
        }
        stage("Pushing image to docker") {
            steps {
                script {
                    sh 'docker push ajagadis/645'
                }
            }
        }
        stage("UpdateDeployment") {
            steps {
                sh 'kubectl rollout restart deploy deploy'
            }
        }
    }
}

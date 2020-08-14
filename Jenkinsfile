pipeline {
    agent any 
    
    script {
      System.setProperty("org.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL", "86400");
    }
    
    environment {
      PROJECT = "cnaps-project"
      APP_NAME = "gateway"
      CLUSTER = "jenkins-cd"
      CLUSTER_ZONE = "asia-northeast3-a"
      IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
      JENKINS_CRED = "${PROJECT}"
    }
    
    stages {
        stage('Stage 0') {
            steps {
                echo 'Hello world!' 
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
                sh "rm -rf src/test"
            }
        }
        stage('Build and Push') {
            steps {
                echo "${IMAGE_TAG}"
                sh "./mvnw package -Pprod -DskipTests jib:build -Dimage=${IMAGE_TAG}"
            }
        }
    }
}

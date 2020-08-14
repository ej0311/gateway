pipeline {
    environment {
      PROJECT = "cnaps-project"
      APP_NAME = "gateway"
      CLUSTER = "jenkins-cd"
      CLUSTER_ZONE = "asia-northeast3-a"
      IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
      JENKINS_CRED = "${PROJECT}"
    }
    
    agent {
        kubernetes {
            label 'build-service'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    job: build-service
spec:
  containers:
  - name: maven
    image: maven:3.6.0-jdk-11-slim
    command: ["cat"]
    tty: true
    volumeMounts:
    - name: cd-jenkins
      mountPath: /root/.m2/repository
  volumes:
  - name: cd-jenkins
    persistentVolumeClaim:
      claimName: cd-jenkins
"""
        }
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
                container('maven') {
                    sh "mvn package -Pprod -DskipTests jib:build -Dimage=${IMAGE_TAG}"
                }
            }
        }
    }
}

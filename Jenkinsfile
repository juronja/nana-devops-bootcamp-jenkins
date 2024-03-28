pipeline {
    agent any
    tools {
        maven "Maven 3.9"
    }
    environment {
        DOCKERHUB_CREDS = credentials('docker-hub-login')
        NEXUS_CREDS = credentials('nexus-login')
    }
    stages {
        stage('Build Jar') {
            steps {
                echo "Building Jar ..."
                sh "mvn package"
            }
        }
        stage('Build Docker') {
            steps {
                echo "Building Docker ..."
                sh "docker build -t juronja/java-maven-app:1.1 ."
                withCredentials([usernamePassword(credentialsId: "docker-hub-login", passwordVariable: "PASS", usernameVariable: "USER" )])
                sh "echo $PASS | docker login -u $USER --password-stdin 64.226.97.173:8082"
                sh "docker push juronja/java-maven-app:1.1"
            }
        }
        stage('Build Docker Nexus') {
            steps {
                echo "Building Docker Nexus ..."
                sh "docker build -t 64.226.97.173:8082/java-maven-app:1.1 ."
                sh "echo $NEXUS_CREDS_PSW | docker login -u $NEXUS_CREDS_USR --password-stdin 64.226.97.173:8082"
                sh "docker push 64.226.97.173:8082/java-maven-app:1.0"
            }
        }
    }
}
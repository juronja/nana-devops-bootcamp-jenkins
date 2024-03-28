pipeline {
    agent any
    tools {
        maven "Maven 3.9"
    }
    
    stages {
        stage('Build Jar') {
            steps {
                echo "Building Jar ..."
                sh "mvn package"
            }
        }
        stage('Build Docker') {
            environment {
                DOCKERHUB_CREDS = credentials('docker-hub-login')
            }
            steps {
                echo "Building Docker ..."
                sh "docker build -t juronja/java-maven-app:1.1 ."
                // Next line in single quotes for security
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
                sh "docker push juronja/java-maven-app:1.1"
            }
        }
        stage('Build Docker Nexus') {
            environment {
               NEXUS_CREDS = credentials('nexus-login')
            }
            steps {
                echo "Building Docker Nexus ..."
                sh "docker build -t 64.226.97.173:8082/java-maven-app:1.1 ."
                // Next line in single quotes for security
                sh 'echo $NEXUS_CREDS_PSW | docker login -u $NEXUS_CREDS_USR --password-stdin 64.226.97.173:8082'
                sh "docker push 64.226.97.173:8082/java-maven-app:1.1"
            }
        }
    }
}
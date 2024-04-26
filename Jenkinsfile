pipeline {
    agent any
    tools {
        maven "Maven 3.9"
    }
    environment {
    BUILD_VERSION = VersionNumber (versionNumberString: '${BUILD_YEAR}.${BUILD_MONTH}.${BUILDS_TODAY}')
    //JOB_NAME
    }

    
    stages {
        stage('Increment Version') {
            steps {
                echo "Incrementing version ... $BUILD_VERSION"
                sh "mvn versions:set -DnewVersion=\\\${BUILD_VERSION} versions:commit"
                
            }
        }
        
        stage('Build Jar') {
            steps {
                echo "Building Jar ..."
                sh "mvn clean package" // Adding clean removes any old .jar packages in target folder
            }
        }
        stage('Build Docker') {
            environment {
                DOCKERHUB_CREDS = credentials('dockerhub-creds')
            }
            steps {
                echo "Building Docker ..."
                sh "docker build -t juronja/$JOB_NAME:$BUILD_VERSION ."
                // Next line in single quotes for security
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
                sh "docker push juronja/$JOB_NAME:$BUILD_VERSION"
            }
        }
//        stage('Build Docker Nexus') {
//            environment {
//                NEXUS_CREDS = credentials('nexus-creds')
//            }
//            steps {
//                echo "Building Docker Nexus ..."
//                sh "docker build -t 64.226.97.173:8082/$JOB_NAME:1.1 ."
//                // Next line in single quotes for security
//                sh 'echo $NEXUS_CREDS_PSW | docker login -u $NEXUS_CREDS_USR --password-stdin 64.226.97.173:8082'
//                sh "docker push 64.226.97.173:8082/$JOB_NAME:1.1"
//            }
//        }
    }
}
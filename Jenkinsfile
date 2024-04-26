pipeline {
    agent any
    tools {
        maven "Maven 3.9"
    }
    
    stages {
        stage('Increment Version') {
            steps {
                script {
                    echo "Incrementing version ..."
                    sh "mvn build-helper:parse-version version:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit"
                    def matcher = readFile("pom.xml") =~ "<version>(.+)</version>"
                    def version = matcher[0][1]
                    env.IMAGE_VERSION = "$version-$BUILD_NUMBER"
                }
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
                sh "docker build -t juronja/java-maven-app:${IMAGE_VERSION} ."
                // Next line in single quotes for security
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
                sh "docker push juronja/java-maven-app:${IMAGE_VERSION}"
            }
        }
//        stage('Build Docker Nexus') {
//            environment {
//                NEXUS_CREDS = credentials('nexus-creds')
//            }
//            steps {
//                echo "Building Docker Nexus ..."
//                sh "docker build -t 64.226.97.173:8082/java-maven-app:1.1 ."
//                // Next line in single quotes for security
//                sh 'echo $NEXUS_CREDS_PSW | docker login -u $NEXUS_CREDS_USR --password-stdin 64.226.97.173:8082'
//                sh "docker push 64.226.97.173:8082/java-maven-app:1.1"
//            }
//        }
    }
}
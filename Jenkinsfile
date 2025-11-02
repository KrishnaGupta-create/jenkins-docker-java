pipeline {
    agent any

    environment {
        // Maven Docker image to use for building
        MAVEN_IMAGE = 'maven:3.9.6-eclipse-temurin-17'
    }

    stages {

        stage('Checkout') {
            steps {
                // Pull your repository from GitHub
                git branch: 'main',
                    url: 'https://github.com/KrishnaGupta-create/jenkins-docker-java.git',
                    credentialsId: 'faddabb5-925f-4e84-9b88-a634be3130d6'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.image(MAVEN_IMAGE).inside {
                        // Compile and package Java project
                        sh 'mvn clean package -DskipTests'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image from Dockerfile
                    sh 'docker build -t jenkins-docker-java:latest .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run --rm -d -p 8080:8080 --name javaapp jenkins-docker-java:latest'
                }
            }
        }
    }

    post {
        always {
            script {
                // Stop and clean up container if running
                sh 'docker ps -q --filter "name=javaapp" | grep -q . && docker stop javaapp || true'
            }
            echo "Pipeline completed."
        }
        failure {
            echo "Pipeline failed ❌"
        }
        success {
            echo "Pipeline succeeded ✅"
        }
    }
}

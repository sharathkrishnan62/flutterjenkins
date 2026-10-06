pipeline {
    agent any

    environment {
        // Runs commands inside an official Flutter container using your local Docker daemon
        DOCKER_CMD = "docker run --rm -v ${WORKSPACE}:/app -w /app ghcr.io/cirruslabs/flutter:stable"
    }

    stages {
        stage('Environment Check') {
            steps {
                sh "${DOCKER_CMD} flutter --version"
                sh "${DOCKER_CMD} flutter doctor -v"
            }
        }

        stage('Install Dependencies') {
            steps {
                sh "${DOCKER_CMD} flutter clean"
                sh "${DOCKER_CMD} flutter pub get"
            }
        }

        stage('Analyze & Lint') {
            steps {
                sh "${DOCKER_CMD} flutter analyze"
            }
        }

        stage('Test') {
            steps {
                sh "${DOCKER_CMD} flutter test"
            }
        }

        stage('Build') {
            steps {
                sh "${DOCKER_CMD} flutter build apk --release"
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/app/outputs/flutter-apk/*.apk', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            cleanWs deleteDirs: true, notFailBuild: true
        }
        success {
            echo 'Flutter pipeline completed successfully!'
        }
        failure {
            echo 'Build failed. Review logs above.'
        }
    }
}
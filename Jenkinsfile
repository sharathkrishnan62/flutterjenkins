pipeline {
    agent any

    environment {
        // Points to the Flutter SDK path on your Jenkins node/server
        PATH = "/opt/flutter/bin:${env.PATH}"
    }

    stages {
        stage('Environment Check') {
            steps {
                sh 'flutter --version'
                sh 'flutter doctor -v'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Fetching Flutter dependencies...'
                sh 'flutter clean'
                sh 'flutter pub get'
            }
        }

        stage('Analyze & Lint') {
            steps {
                echo 'Checking code style and potential errors...'
                sh 'flutter analyze'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit/widget tests...'
                sh 'flutter test --coverage'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                // Build an Android APK (change to 'flutter build appbundle' or 'flutter build web' as needed)
                sh 'flutter build apk --release'
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archives the generated APK so you can download it directly from Jenkins
                archiveArtifacts artifacts: 'build/app/outputs/flutter-apk/*.apk', fingerprint: true
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
            echo 'Flutter build/test failed. Check logs above.'
        }
    }
}
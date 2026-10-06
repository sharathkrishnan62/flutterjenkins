pipeline {
    agent {
        docker {
            image 'ghcr.io/cirruslabs/flutter:stable'
            // Maps the Gradle/Pub caches to persist between runs
            args '-u root -v /tmp/.gradle:/root/.gradle -v /tmp/.pub-cache:/root/.pub-cache'
        }
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
                sh 'flutter clean'
                sh 'flutter pub get'
            }
        }

        stage('Analyze & Lint') {
            steps {
                sh 'flutter analyze'
            }
        }

        stage('Test') {
            steps {
                sh 'flutter test'
            }
        }

        stage('Build') {
            steps {
                // If building an APK:
                sh 'flutter build apk --release'
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
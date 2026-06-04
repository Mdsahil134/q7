pipeline {
    agent any

    parameters {
        string(
            name: 'BRANCH_NAME',
            defaultValue: 'main',
            description: 'Git branch to build'
        )

        string(
            name: 'APP_VERSION',
            defaultValue: '1.0',
            description: 'Application version'
        )
    }

    environment {
        BUILD_DIR = "target"
        ARTIFACT_NAME = "myapp-${APP_VERSION}.jar"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}",
                    url: 'https://github.com/user/my-app.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building Version ${params.APP_VERSION}"
                bat 'mvn clean compile'
            }
        }

        stage('Unit Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Code Quality Check') {
            steps {
                bat 'mvn checkstyle:check'
            }
        }

        stage('Package') {
            steps {
                bat "mvn package -Dversion=${params.APP_VERSION}"
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar',
                                  fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build Successful'
        }

        failure {
            echo 'Build Failed'
        }
    }
}
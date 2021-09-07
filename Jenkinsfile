pipeline {
    agent any

    stages {

        stage('Get Source') {
            steps {
                git url: 'https://github.com/emvnuel/ci-cd-test.git', branch: 'main'

            }
        }

        stage('Build') {
            steps {
                ./mvnw clean compile install -DskipTests
            }
        }

        stage('test') {
            steps {
                ./mvnw validate test
            }
        }
    }
}
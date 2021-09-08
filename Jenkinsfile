pipeline {
    agent any

    stages {

        stage('Get Source') {
            steps {
                git url: 'https://github.com/emvnuel/ci-cd-test.git', branch: 'main'

            }
        }

        stage('Build'){
            steps {
                sh './mvnw clean compile install -DskipTests'
            }
        }

        
        stage('Test'){
            steps {
                sh './mvnw validate test'
            }
            
        }

    }
}
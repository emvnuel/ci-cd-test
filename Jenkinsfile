pipeline {
    agent any

    stages {

        stage('Get Source') {
            steps {
                git url: 'https://github.com/emvnuel/ci-cd-test.git', branch: 'main'

            }
        }

        stage('Build'){
            withMaven(maven: 'mvn') {
                sh "mvn clean compile install -DskipTests"
            }
        }

        
        stage('Test'){
            withMaven(maven: 'mvn') {
                sh "mvn validate test"
            }
            
        }

    }
}
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
                withMaven(maven: 'mvn') {
                    sh "mvn clean compile install -DskipTests"
                }
            }
        }

        
        stage('Test'){
            steps {
                withMaven(maven: 'mvn') {
                    sh "mvn validate test"
                }
            }
            
        }

    }
}
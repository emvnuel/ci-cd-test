pipeline {
    agent any

    stages {

        stage('Get Source') {
            steps {
                git url: 'https://github.com/emvnuel/ci-cd-test.git', branch: 'main'

            }
        }

        stage('Build') {

            withMaven {
                sh "mvn clean compile install -DskipTests"
            }

        }

        stage('test') {
            withMaven {
                sh "mvn validate test"
            }
        }
    }
}
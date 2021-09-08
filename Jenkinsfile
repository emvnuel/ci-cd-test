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

        stage('Docker Build') {
            steps  {
                script {
                    dockerapp = docker.build("myawesomeapps/petclinic:${env.BUILD_ID}",
                        '-f ./Dockerfile .')
                }
            }
        }

        stage('Docker Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    }
}
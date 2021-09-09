#!groovy
pipeline {

    agent any
    environment {
        PROJECT_ID = credentials('project_id')
        LOCATION = credentials('cluster_location')
        CREDENTIALS_ID = 'gke'
        CLUSTER_NAME = credentials('cluster_name')
    }
    
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

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar'
                }
            }
        }

        stage('Docker Build') {
            steps {
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

    	stage('GKE Deployment') {
      		steps{
                step([$class: 'KubernetesEngineBuilder', 
                      projectId: env.PROJECT_ID, 
                      clusterName: env.CLUSTER_NAME, 
                      location: env.LOCATION, 
                      manifestPattern: './k8s', 
                      credentialsId: env.CREDENTIALS_ID, 
                      verifyDeployments: true])
            }
    	}

    }

}

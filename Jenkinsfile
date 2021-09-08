#!groovy
node {

    stage('Get Source') {
        git url: 'https://github.com/emvnuel/ci-cd-test.git', branch: 'main'
    }

    stage('Build'){
        sh './mvnw clean compile install -DskipTests'
    }

    
    stage('Test'){
        sh './mvnw validate test'
    }

    stage('Docker Build') {
        script {
            dockerapp = docker.build("myawesomeapps/petclinic:${env.BUILD_ID}",
                '-f ./Dockerfile .')
        }
    }

    stage('Docker Push Image') {
        script {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                dockerapp.push('latest')
                dockerapp.push("${env.BUILD_ID}")
            }
        }
    }

}
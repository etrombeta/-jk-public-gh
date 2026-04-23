pipeline {
    agent any
    environment {
    DOCKERHUB_CREDENTIALS = credentials('jk-dh-pat')
}

    stages {
        stage('Clonning Git Repository') {
            steps {
                git branch: 'main', credentialsId: 'jk-gh-pat', url: 'https://github.com/etrombeta/jk-private-gh.git'
            }
        }
        stage('Building Docker Image') {
            steps {
                sh 'docker build -t etrombeta/webapp:$BUILD_NUMBER .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Pushing Image') {
            steps {
                sh 'docker push etrombeta/webapp:$BUILD_NUMBER'
            }
        }
        stage('Deploying Application') {
            steps {
                sh '''
                # docker stop webapp_ctr
                docker run --rm -d -p 3000:3000 --name webapp_ctr etrombeta/webapp:$BUILD_NUMBER
                
             '''   
            }
        }
        
    }
}

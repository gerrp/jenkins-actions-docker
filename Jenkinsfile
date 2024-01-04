pipeline {
    agent any
    environment{
        DOCKERHUB_CREDENCIALS = credentials ('dockerhub')
        RepoDockerHub = 'gerrp'
        NameContainer = 'pythonflask'
    }

    stages {
        stage('Build'){
            steps{
                sh "docker build -t ${env.RepoDockerHub}/${env.NameContainer}:${env.BUILD_NUMBER} ."
            }
        }

        stage('Install Hadolint'){
            steps{
                sh 'mkdir -p ~/bin'
                sh 'wget https://github.com/hadolint/hadolint/releases/download/v2.12.0/hadolint-Linux-x86_64 -O ~/bin/hadolint'
                sh 'chmod +x ~/bin/hadolint'
            }
        }

        stage('Lint Dockerfile'){
            steps{
                script{
                def hadolintExitCode = sh(script: "~/bin/hadolint /home/vagrant/lolflask/dockerfile", returnStatus: true) 
                if (hadolintExitCode !=0) {
                    currentBuild.result = 'FAILURE'
                    error("Hadolint found issues in the dockerfile")
                }
            }
        }
        }

        stage('Login to Dockerhub'){
            steps{
                sh "echo $DOCKERHUB_CREDENCIALS_PSW | docker login -u $DOCKERHUB_CREDENCIALS_USR --password-stdin "
            }
        }
    
        stage('Push image to Dockerhub'){
            steps{
                sh "docker push ${env.RepoDockerHub}/${env.NameContainer}:${env.BUILD_NUMBER} "
            }
        }

         stage('Deploy container'){
            steps{
                sh "if [ 'docker stop ${env.NameContainer}' ] ; then docker rm -f ${env.NameContainer} && docker run -d --name ${env.NameContainer} -p 5000:5000 ${env.RepoDockerHub}/${env.NameContainer}:${env.BUILD_NUMBER} ; else docker run -d --name pokedex-flask -p 5000:5000 ${env.RepoDockerHub}/${env.NameContainer}:${env.BUILD_NUMBER} ; fi"
            }
        }


        stage('Verify application') {
            steps {
                script{
                 sh 'sleep 15'
                def response = httpRequest 'http://localhost:5000'
                if (response.status==200){
                    echo 'Application responded with HTTP status 200. Verification passed.'
                    } else {
                    currentBuild.result = 'FAILURE'
            
                    }
                }
            }
        }
        
        stage('Docker logout'){
            steps{
                sh "docker logout"
            }
        }

    } 

}   

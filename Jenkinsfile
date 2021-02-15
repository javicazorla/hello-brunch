#!/usr/bin/env groovy

pipeline {
    agent any
    options {
        ansiColor('xterm')
    }

<<<<<<< HEAD
    stages {
        stage("Trivy Scan"){
            steps{
                sh 'trivy image --format json --output trivy-results.json node:lts-buster'
            }
            post {
                always {
                    recordIssues(
                    enabledForFailure: true,
                    tool: trivy(pattern: '*.json')
                    )
                }
            }
        }
        
=======
    stages { 
>>>>>>> 5c96d7d64c09dc5741a55b5eb86f9c66f491260f
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }
        
        stage("Trivy Scan"){
            steps{
                sh 'trivy filesystem -f json -o trivy-fs.json .'
                sh 'trivy image --format json --output trivy-image.json hello-brunch'
            }
            post {
                always {
                    recordIssues(
                    enabledForFailure: true,
                    aggregatingResults: true,
                    tool: trivy(pattern: 'trivy-*.json')
                    )
                }
            }
        }
        
        stage('Publish') {
            steps {
                withDockerRegistry([
                    credentialsId:"gitlab-registry",
                    url:"http://10.250.4.2:5050"
                ]){
                    sh 'docker tag hello-brunch:latest 10.250.4.2:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
                    sh 'docker push 10.250.4.2:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
                }
            }
        }
    }
}

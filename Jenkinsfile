#!/usr/bin/env groovy

pipeline {
    agent any
    options {
        ansiColor('xterm')
    }

    stages { 
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
        
        stage('Deploy') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}

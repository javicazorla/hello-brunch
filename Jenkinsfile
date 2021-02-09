#!/usr/bin/env groovy

pipeline {
    agent any
    options {
        ansiColor('xterm')
    }

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
        
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}

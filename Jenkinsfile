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
        stage('Deploy') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}

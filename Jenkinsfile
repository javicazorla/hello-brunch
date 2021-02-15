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
        
        stage('Publish') {
            steps {
                withDockerRegistry([
                    credentialsId:"gitlab-registry",
                    url:"http://10.250.4.2:5050"
                ]){
                    sh 'docker tag hello-brunch:latest 10.250.4.2:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
                    sh 'docker push 10.250.4.2:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
                    sh 'docker tag hello-brunch:latest 10.250.4.2:5050/root/hello-brunch:latest'
                    sh 'docker push 10.250.4.2:5050/root/hello-brunch:latest'
                    sshagent(['ssh-github']) {
                        sh 'git tag BUILD-1.${BUILD_NUMBER}'
                        sh 'git push --tags'
                    }
                }
            }
        }
    }
}

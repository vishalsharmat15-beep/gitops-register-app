pipeline {
    agent { label 'Jenkins-Agent' }
    environment {
        APP_NAME = "register-app"
    }
    parameters {
    string(name: 'IMAGE_TAG', description: 'Docker image tag')
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/vishalsharmat15-beep/gitops-register-app.git'
            }
        }

        stage("Update Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i "/image:.*${APP_NAME}/s/:[^:]*\\$/:${IMAGE_TAG}/" deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changes to Git") {
            steps {
                sh """
                    git config --global user.name "vishalsharmat15-beep"
                    git config --global user.email "vishalsharmat15-beep@example.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/vishalsharmat15-beep/gitops-register-app.git main"
                }
            }
        }
    
    
    }
}
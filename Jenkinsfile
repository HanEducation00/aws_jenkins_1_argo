pipeline {
    agent {
        label 'My-Jenkins-Agent'
    }
    
    environment {
        APP_NAME = "aws_jenkins_1_argo"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/HanEducation00/aws_jenkins_1_argo.git'
            }
        }
        
        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "HanEducation00"
                   git config --global user.email "han.oguz.education@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest" --allow-empty
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/HanEducation00/aws_jenkins_1_argo.git main" 
                }
            }
        }
    }
}

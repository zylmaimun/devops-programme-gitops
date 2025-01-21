pipeline {
    agent any
    environment {
          APP_NAME = "devops-programme-app"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'github', url: 'https://github.com/zylmaimun/devops-programme-gitops'
             }
         }
         stage("Update Deployment Tags") {
             steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
             }
         }
         stage("Push updated deployment file to GitHub") {
             steps {
                sh """
                    git config --global user.name "zylmaimun"
                    git config --global user.email "simeon.k.sabev@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/zylmaimun/devops-programme-gitops main"
                }
             }
         }
    }
}

pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace"){
            steps {
                cleasWs()
            }
        }

        stage("Checkout from SCM"){
            git branch: 'main', credentialsId: 'github', url: 'https://github.com/testjay925/gitops-register-app'
        }

        stage("Update the Deployment Tags"){
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Pushed changed deployment file to Git"){
            sh """
                git config --global user.name "testjay925"
                git config --global user.email "jay.oswal@spit.ac.in"
                git add deployment.yaml
                git commit -m "Updated Deployment Manifest"
            """

            withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                sh "git push https://github.com/testjay925/gitops-register-app main"
            }
        }
    }
}

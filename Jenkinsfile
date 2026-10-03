pipeline {
    agent { label "Jenkins-Agent" }

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag passed from CI pipeline')
    }

    environment {
        APP_NAME = "register-app"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/rahulgpt2202/gitops-register-app-cd'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${params.IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "rahulgpt2202"
                    git config --global user.email "rahulgpt2202@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://${GIT_TOKEN}@github.com/rahulgpt2202/gitops-register-app-cd.git main"
                }
            }
        }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

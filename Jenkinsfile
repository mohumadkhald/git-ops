pipeline
{
    agent any

    environment {
        APP_NAME = "e2e-pipeline-ci_cd"
        

    }

    stages {
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'Github', url: 'https://github.com/mohumadkhald/git-ops'
            }
        }

        stage("update App Deployment"){
            steps {
                sh '''
                cat ./dev/deployment.yml
                sed -i "s/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g" ./dev/deployment.yml
                cat ./dev/deployment.yml
                '''
            }
        }
        stage("Commit and Push Changes") {
            steps {
                withCredentials([usernamePassword(credentialsId: "Github", usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                        git config user.email "jenkins@yourdomain.com"
                        git config user.name "Jenkins"

                        git add ./dev/deployment.yml
                        git commit -m "Update ${APP_NAME} image to ${IMAGE_TAG}"
                        git push https://${USER}:${PASS}@github.com/mohumadkhald/git-ops.git main
                    '''
                }
            }
        }

    }
}

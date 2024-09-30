pipeline
{
    agent any

    environment {
        APP_NAME = "e2e-pipeline-ci_cd"
        GIT_CREDENTIALS = 'Dockerhub'  // Replace with your GitHub credentials ID
        GIT_BRANCH = 'main'         // Branch to push to
        GIT_URL = 'https://github.com/mohumadkhald/git-ops'

    }

    stages {
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM"){
            steps {
                git branch: GIT_BRANCH, credentialsId: 'Github', url: GIT_URL
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
                withCredentials([usernamePassword(credentialsId: GIT_CREDENTIALS, usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                        git config user.email "jenkins@gmail.com"
                        git config user.name "Jenkins"

                        git add ./dev/deployment.yml
                        git commit -m "Update ${APP_NAME} image to ${IMAGE_TAG}"
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/mohumadkhald/git-ops ${GIT_BRANCH}
                    '''
                }
            }
        }

    }
}



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
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' ./dev/deployment.yml
                    cat ./dev/deployment.yml
                  '''
              }
          }
          

    }
}

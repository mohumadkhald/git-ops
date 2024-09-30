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
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' ./dev/deploymetn.yml
                    cat ./dev/deployment.yml
                  '''
              }
          }

    }
}

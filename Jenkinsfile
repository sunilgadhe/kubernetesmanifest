node {
    def app

    stage('Clone repository') {
        checkout scm
        sh "git checkout main"  
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh """
                        git config user.email 'sunilgade.cloud@gmail.com'
                        git config user.name 'sunilgadhe'
                        cat deployment.yaml
                        sed -i 's+sunilgadhe/test.*+sunilgadhe/test:${DOCKERTAG}+g' deployment.yaml
                        cat deployment.yaml
                        git add deployment.yaml
                        git commit -m 'Done by Jenkins Job changemanifest: ${BUILD_NUMBER}' || echo 'No changes to commit'
                        git remote set-url origin https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git
                        git pull --rebase origin main
                        git push origin main
                    """
                }
            }
        }
    }
}

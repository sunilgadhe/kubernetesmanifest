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
                        # Always start clean
                        git fetch origin
                        git reset --hard origin/main

                        # Update deployment.yaml
                        sed -i s+sunilgadhe/test.*+sunilgadhe/test:26+g deployment.yaml

                        # Commit only if there are changes
                        if git diff --quiet; then
                          echo "No changes to commit"
                        else
                          git add deployment.yaml
                          git commit -m "Done by Jenkins Job changemanifest: ${BUILD_NUMBER}"
                          git push https://sunilgadhe:${GIT_PASSWORD}@github.com/sunilgadhe/kubernetesmanifest.git
                        fi
                    """
                }
            }
        }
    }
}

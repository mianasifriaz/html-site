pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                sshagent(credentials: ['html-site-ssh']) {
                    git url: 'git@github.com:mianasifriaz/html-site.git', branch: 'main'
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['html-site-ssh']) {
                    sh '''
                    # Define remote paths
                    REMOTE_USER=html-site
                    REMOTE_HOST=lb-nginx
                    DEPLOY_PATH=/home/html-site/public_html
                    TMP_PATH=/home/html-site/public_html_tmp
                    BACKUP_PATH=/home/html-site/public_html_old

                    # Step 1: Sync to temp directory
                    rsync -avz --delete --exclude=".git" --exclude="Jenkinsfile" ./ ${REMOTE_USER}@${REMOTE_HOST}:${TMP_PATH}/

                    # Step 2: Atomically switch to new version
                    ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} bash -c "'
                        rm -rf ${BACKUP_PATH}
                        mv ${DEPLOY_PATH} ${BACKUP_PATH} || true
                        mv ${TMP_PATH} ${DEPLOY_PATH}
                        chown -R html-site:html-site ${DEPLOY_PATH}
                    '"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Site deployed successfully.'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}

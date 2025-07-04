pipeline {
    agent any

    environment {
        DEPLOY_USER = 'html-site'
        DEPLOY_HOST = 'lb-nginx'
        DEPLOY_DIR = '/home/html-site/public_html'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'git@github.com:mianasifriaz/html-site.git', branch: 'main'
            }
        }

        stage('Deploy via SSH') {
            steps {
                sshagent (credentials: ['html-site-ssh']) {
                    sh '''
                    echo "Deploying to $DEPLOY_HOST:$DEPLOY_DIR ..."

                    # Sync all files safely using rsync
                    rsync -avz \
                        --exclude=".git/" \
                        --exclude="README.md" \
                        --exclude="Jenkinsfile" \
                        --exclude="awstats/" \
                        --exclude="awstats-icon/" \
                        --exclude="awstatsicons/" \
                        --exclude="icon/" \
                        ./ $DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_DIR/

                    echo "Deployment completed successfully."
                    '''
                }
            }
        }
    }

    post {
        failure {
            echo 'Deployment failed.'
        }
        success {
            echo 'Site deployed successfully.'
        }
    }
}


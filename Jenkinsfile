pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'git@github.com:mianasifriaz/html-site.git', branch: 'main'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Use rsync or cp to copy files to /var/www/html-site
                    sh '''
                        rm -rf /var/www/html-site/*
                        cp -r * /var/www/html-site/
                        chown -R www-data:www-data /var/www/html-site
                    '''
                }
            }
        }
    }
}


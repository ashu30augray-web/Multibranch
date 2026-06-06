pipeline {

    agent any

    environment {

        DEV_SERVER = "18.231.225.217"
        STG_SERVER = "18.229.142.121"
        PRD_SERVER = "18.229.142.121"

        SERVER_USER = "ec2-user"
    }

    stages {

        stage('Checkout') {

            steps {

                checkout scm
            }
        }

        stage('Deploy to DEV') {

            when {

                branch 'dev'
            }

            steps {

                sh """

                echo "Deploying to DEV Server"

                scp -o StrictHostKeyChecking=no \
                index.html ${SERVER_USER}@${DEV_SERVER}:/tmp/index.html

                ssh -o StrictHostKeyChecking=no \
                ${SERVER_USER}@${DEV_SERVER} '

                sudo cp /tmp/index.html /var/www/html/index.html

                sudo systemctl restart httpd
                '

                """
            }
        }

        stage('Deploy to STAGING') {

            when {

                branch 'stage'
            }

            steps {

                sh """

                echo "Deploying to STAGING Server"

                scp -o StrictHostKeyChecking=no \
                index.html ${SERVER_USER}@${STG_SERVER}:/tmp/index.html

                ssh -o StrictHostKeyChecking=no \
                ${SERVER_USER}@${STG_SERVER} '

                sudo cp /tmp/index.html /var/www/html/index.html

                sudo systemctl restart httpd
                '

                """
            }
        }

        stage('Deploy to PRODUCTION') {

            when {

                branch 'main'
            }

            steps {

                sh """

                echo "Deploying to PRODUCTION Server"

                scp -o StrictHostKeyChecking=no \
                index.html ${SERVER_USER}@${PRD_SERVER}:/tmp/index.html

                ssh -o StrictHostKeyChecking=no \
                ${SERVER_USER}@${PRD_SERVER} '

                sudo cp /tmp/index.html /var/www/html/index.html

                sudo systemctl restart httpd
                '

                """
            }
        }
    }

    post {

        success {

            echo "Deployment Successful"
        }

        failure {

            echo "Deployment Failed"
        }
    }
}

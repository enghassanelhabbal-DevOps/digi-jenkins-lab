pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/elashrypublic/digi-jenkins-lab-week-14.git'
            }
        }

        stage('Install & Test (Docker)') {
            agent {
                docker {
                    image 'php:8.2-cli'
                }
            }

            steps {
                sh 'php -v'

                sh 'apt-get update && apt-get install -y unzip curl git'

                sh 'curl -sS https://getcomposer.org/installer | php'
                sh 'php composer.phar install'

                sh './vendor/bin/phpunit --log-junit results.xml'
            }
        }

        stage('Display Results') {
            steps {
                junit 'results.xml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline job finished.'
        }
        success {
            echo 'All tests passed successfully.'
        }
        failure {
            echo 'Tests failed. Check results.'
        }
    }
}
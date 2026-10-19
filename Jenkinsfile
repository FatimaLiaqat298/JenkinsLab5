pipeline {
    agent any

    stages {
        stage('Checkout Source Code') {
            steps {
                echo 'Fetching code from GitHub repository...'
                git branch: 'main', url: 'https://github.com/FatimaLiaqat298/JenkinsLab5.git'
            }
        }

        stage('Build Dependencies') {
            steps {
                echo 'Installing PHP and Composer dependencies...'
                sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
                
                echo 'Installing frontend assets...'
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running application tests...'
                // Agar aapke paas unit tests hain toh yeh run honge
                sh 'php artisan test'
            }
        }

        stage('Database Migration') {
            steps {
                echo 'Setting up environment and running database migrations...'
                sh '''
                    if [ ! -f .env ]; then
                        cp .env.example .env
                        php artisan key:generate
                    fi
                '''
                sh 'php artisan migrate --force'
            }
        }

        stage('Deploy & Optimize') {
            steps {
                echo 'Optimizing application for production deployment...'
                sh '''
                    php artisan config:cache
                    php artisan route:cache
                    php artisan view:cache
                '''
                echo 'Deployment sequence completed successfully!'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully across all stages (Build, Test, Deploy)!'
        }
        failure {
            echo 'Pipeline failed during execution. Check console output.'
        }
    }
}

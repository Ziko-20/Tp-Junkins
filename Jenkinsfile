pipeline {
    agent none // On ne définit pas d'agent global, on le fera par étape (stage)

    environment {
        // Variables d'environnement pour Laravel
        APP_ENV = 'testing'
        BCRYPT_ROUNDS = '4'
        CACHE_DRIVER = 'array'
        SESSION_DRIVER = 'array'
        QUEUE_CONNECTION = 'sync'
        DB_CONNECTION = 'sqlite'
        DB_DATABASE = ':memory:'
    }

    stages {
        stage('Initialisation PHP') {
            agent {
                docker { 
                    image 'chialab/php:8.2-fpm' // Image PHP contenant déjà Composer et les extensions Laravel
                    args '-u root'
                }
            }
            steps {
                echo 'Installation des dépendances PHP...'
                sh 'composer install --no-ansi --no-interaction --no-scripts --progress=false --prefer-dist'
                
                echo 'Configuration du fichier .env de test...'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }

        stage('Initialisation Frontend') {
            agent {
                docker { image 'node:18-alpine' }
            }
            steps {
                echo 'Installation de Node et build des assets...'
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('Tests Unitaires') {
            agent {
                docker { 
                    image 'chialab/php:8.2-fpm'
                    args '-u root'
                }
            }
            steps {
                echo 'Exécution des tests PHPUnit...'
                sh './vendor/bin/phpunit'
            }
        }
    }

    post {
        always {
            echo 'Nettoyage ou archivage des résultats si nécessaire.'
        }
        success {
            echo 'Félicitations ! Le build Laravel est un succès.'
        }
        failure {
            echo 'Le build a échoué. Vérifiez les logs des tests.'
        }
    }
}
pipeline {
    agent {
        docker { 
            image 'chialab/php:8.2-fpm'
            args '-u root'
        }
    }

    environment {
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
            steps {
                echo 'Installation des dépendances PHP...'
                // La correction est bien appliquée ici sans conflit
                sh 'composer install --no-ansi --no-interaction --no-scripts --no-progress --prefer-dist'
                
                echo 'Configuration du fichier .env de test...'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }

        stage('Tests Unitaires') {
            steps {
                echo 'Exécution des tests PHPUnit...'
                sh './vendor/bin/phpunit'
            }
        }
    }

    post {
        always {
            echo 'Nettoyage des résultats.'
        }
        success {
            echo 'Félicitations ! Le build Laravel est un succès.'
        }
        failure {
            echo 'Le build a échoué.'
        }
    }
}
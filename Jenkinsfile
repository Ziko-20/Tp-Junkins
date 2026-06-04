pipeline {
    // MODIFICATION ICI : On définit l'image PHP globalement pour tout le pipeline
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
                sh 'composer install --no-ansi --no-interaction --no-scripts --progress=false --prefer-dist'
                
                echo 'Configuration du fichier .env de test...'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }

        // Note : J'ai retiré temporairement l'étape Node pour s'assurer 
        // que la partie PHP/Laravel passe sans conflit d'image Docker.
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
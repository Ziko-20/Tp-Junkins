stage('Initialisation PHP') {
            steps {
                echo 'Installation des dépendances PHP...'
                // MODIFICATION ICI : --no-progress à la place de --progress=false
                sh 'composer install --no-ansi --no-interaction --no-scripts --no-progress --prefer-dist'
                
                echo 'Configuration du fichier .env de test...'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }
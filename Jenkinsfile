pipeline {
    agent any

    environment {
        COMPOSE_FILE = 'docker-compose.yml' // Define el archivo docker-compose a usar
    }

    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio de GitHub
                git branch: 'main', url: 'https://github.com/Vaikra/Com.git' 
            }
        }

        stage('Install Dependencies') {
            steps {
                // Instala dependencias del requirements.txt en un contenedor temporal
                sh 'docker run --rm -v $PWD:/app -w /app python:3.8 pip install -r requirements.txt'
            }
        }

        stage('Build and Start Services') {
            steps {
                // Construye y levanta los contenedores definidos en docker-compose.yml
                sh 'docker-compose up -d --build'
            }
        }

        //stage('Initialize Database') {
            //steps {
                // Carga los datos de data.sql a la base de datos MySQL una vez que el contenedor está listo
                // Usa el contenedor llamado "db_Comisaria" como definido en docker-compose.yml
                //script {
                    // Espera a que el contenedor de MySQL esté en estado healthy
                    //sh 'docker-compose exec db_Comisaria mysql -uroot -ppass -e "SELECT 1"'
                }
                // Carga el archivo SQL en la base de datos
                //sh '''
                //docker-compose exec -T db_Comisaria mysql -uroot -ppass comisaria_db < MySQL/data.sql
                //'''
            //}
        //}

        stage('Run Application') {
            steps {
                echo 'La aplicación Django debería estar en ejecución en el contenedor ComisariaApp'
            }
        }
    }

    post {
        always {
            // Detener y limpiar los contenedores al final del pipeline
            sh 'docker-compose down'
        }
    }
}

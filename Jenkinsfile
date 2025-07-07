pipeline {
    agent any
    // escenarios -> escenario -> pasos
    enviroment{
        NPM_CONFIG_CACHE= "${WORKSPACE}/.npm"
    }
    stages{
        stage ("saludo a usuario") {
            steps {
                sh 'echo "comenzando mi pipeline"'
            }
        }
        stage ("salida de los saludos a usuarios") {
            steps {
                sh 'echo "saliendo de este grupo de usuarios"'
            }
        }
        stage ("proceso de builds y test") {
            agent {
                docker{
                    image 'node:22'
                    reuseNode true
                }
            }
            stages{
                stage ('Instalacion de dependencias'){
                    steps {
                        sh 'npm ci'
                    }
                }
                stage ('ejecucion de pruebas'){
                    steps {
                        sh 'npm run test:cov'
                    }
                }
                stage ('construccion de la aplicacion'){
                    steps {
                        sh 'npm run build'
                    }
                }
            }
        }
        stage ('build y push de imagen docker'){
            steps {
                 sh 'docker build -t backend-nest_cgc .'
            }
        }
    }
}
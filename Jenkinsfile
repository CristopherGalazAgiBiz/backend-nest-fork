pipeline {
    agent any
    // escenarios -> escenario -> pasos
    environment{
        NPM_CONFIG_CACHE= "${WORKSPACE}/.npm"
        dockerImagePrefix = "us-west1-docker.pkg.dev/lab-agibiz/docker-repository"
        registry = "https://us.west1-docker.pkg.dev"
        registryCredentials = "gcp-registry"
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
                //  sh "docker login -u -p ${registry}"
                script{
                    docker.withRegistry("${registry}", registryCredentials){
                        sh "docker build -t backend-nest-cgc ."
                        sh "docker tag backend-nest-cgc ${dockerImagePrefix}/backend-nest-cgc"
                        sh "docker push ${dockerImagePrefix}/backend-nest-cgc"
                    }
                }
            }
        }
    }
}
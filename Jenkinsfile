pipeline {
    agent any
   
    stages {
        stage('Mostrar Información del Entorno') {
            steps {
                echo "=== Información del Entorno de Jenkins ==="
                
                // Mostrar variables de entorno de Jenkins
                sh 'echo "Jenkins Home: $JENKINS_HOME"'
                sh 'echo "Node Name: $NODE_NAME"'
                sh 'echo "Job Name: $JOB_NAME"'
                sh 'echo "Build Number: $BUILD_NUMBER"'

                // Usuario que ejecuta el proceso
                sh 'echo "Usuario del proceso: $(whoami)"'
                
                // Mostrar información del sistema operativo
                sh 'echo "Sistema Operativo:" && uname -a'
                
                // Mostrar directorio de trabajo
                sh 'echo "Directorio de trabajo:" && pwd'
                
                // Mostrar espacio en disco
                sh 'echo "Espacio en disco disponible:" && df -h'
                
                // Mostrar memoria
                sh 'echo "Memoria disponible:" && free -h'
                
                // Mostrar variables de entorno completas (opcional)
                sh 'echo "=== Todas las variables de entorno ===" && printenv'
            }
        }
        stage('Configure Docker remote context') {
            steps {
                echo 'Configuring Docker remote context...'
                sh '''
                  docker context inspect remoto >/dev/null 2>&1 || \
                  docker context create remoto --docker "host=ssh://pi@192.168.2.4"
                  docker context use remoto
                  docker context ls
                '''
            }
        }
        stage('Drop the Apache Tomcat Docker container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f tomcat1'
            }
        }
        stage('Create the Tomcat container') {
            steps {
            echo 'Creating the container...'
            sh 'docker run -dit --name tomcat1 -p 9090:8080  -v $WORKSPACE/tomcat-web:/usr/local/tomcat/webapps tomcat:9.0'
            }
        }
        stage('Copy web files into container') {
            steps {
                sh 'docker cp $WORKSPACE/tomcat-web/. tomcat1:/usr/local/tomcat/webapps'
            }
        }
}

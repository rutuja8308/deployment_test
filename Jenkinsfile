pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'MAVEN'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/rutuja8308/deployment_test.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy WAR') {
            steps {
                bat '''
                del /f /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project.war" || echo "No old WAR"
                copy target\\*.war "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project.war" /Y
                '''
            }
        }

    }
}
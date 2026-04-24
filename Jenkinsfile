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
                bat 'mvn clean package'
            }
        }

        stage('Stop Tomcat') {
            steps {
                bat 'net stop Tomcat10'
            }
        }

        stage('Deploy WAR') {
            steps {
                bat 'copy target\\*.war "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project.war"'
            }
        }

        stage('Start Tomcat') {
            steps {
                bat 'net start Tomcat10'
            }
        }
    }
}

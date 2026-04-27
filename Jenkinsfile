pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Enter branch name')
    }

    tools {
        jdk 'JDK21'
        maven 'MAVEN'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}",
                    url: 'https://github.com/rutuja8308/deployment_test.git'
            }
        }

        stage('Verify Branch') {
            steps {
                bat 'git branch'
                bat 'git log -1'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests -U'
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                net stop Tomcat10

                rmdir /s /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project"
                del /f /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project.war"

                xcopy "%WORKSPACE%\\target\\Project.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\" /Y

                net start Tomcat10
                '''
            }
        }
    }
}
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

        stage('Checkout Code') {
            steps {
                git branch: "${params.BRANCH_NAME}",
                    url: 'https://github.com/rutuja8308/deployment_test.git'
            }
        }

        stage('Build WAR') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat '''
                echo ===== Removing old deployment =====
                del /f /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project.war" || echo No old WAR
                rmdir /s /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project" || echo No old folder

                echo ===== Copying new WAR =====
                xcopy "%WORKSPACE%\\target\\Project.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\" /Y
                '''
            }
        }

    }
}
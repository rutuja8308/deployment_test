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

        stage('Verify Code') {
            steps {
                bat 'echo ===== CURRENT BRANCH ====='
                bat 'git rev-parse --abbrev-ref HEAD'

                bat 'echo ===== LATEST COMMIT ====='
                bat 'git log -1 --oneline'

                bat 'echo ===== FILE LIST ====='
                bat 'dir'
            }
        }

        stage('Build WAR') {
            steps {
                bat 'mvn clean package -DskipTests -U'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat '''
                echo ===== STOP TOMCAT =====
                net stop Tomcat10

                echo ===== DELETE OLD DEPLOYMENT =====
                rmdir /s /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project" || echo No folder
                del /f /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project.war" || echo No WAR

                echo ===== WAIT =====
                timeout /t 5

                echo ===== COPY NEW WAR =====
                xcopy "%WORKSPACE%\\target\\Project.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\" /Y

                echo ===== START TOMCAT =====
                net start Tomcat10
                '''
            }
        }

    }
}
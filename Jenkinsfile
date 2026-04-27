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
                bat 'mvn clean package -DskipTests -U'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat '''
                echo ===== STOP TOMCAT =====
                net stop Tomcat10

                echo ===== FORCE FREE PORT 8080 =====
                for /F "tokens=5" %%a in ('netstat -ano ^| findstr :8080') do (
                    if not "%%a"=="0" taskkill /PID %%a /F
                )

                echo ===== WAIT =====
                ping 127.0.0.1 -n 6 > nul

                echo ===== DELETE OLD DEPLOYMENT =====
                rmdir /s /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project" || echo No folder
                del /f /q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\Project.war" || echo No WAR

                echo ===== COPY NEW WAR =====
                xcopy "%WORKSPACE%\\target\\Project.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\" /Y

                echo ===== START TOMCAT =====
                net start Tomcat10
                '''
            }
        }

        stage('Health Check') {
            steps {
                bat '''
                echo ===== CHECKING APPLICATION =====
                timeout /t 10

                curl -I http://localhost:8080/Project || echo App not reachable
                '''
            }
        }

    }
}
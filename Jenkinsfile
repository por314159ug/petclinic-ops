pipeline {
    agent any
    
    tools {
        maven 'Maven'
        jdk 'Java'
    }
    
    environment {
        JAVA_HOME = tool 'Java'
    }
    
    stages {
        stage('Initialize') {
            steps {
                bat 'echo Pipeline started'
            }
        }
        
        stage('Checkout') {
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic.git',
                branch: 'main'
            }
        }

        stage('Build') {
            steps {
                bat '.\\mvnw.cmd clean package'
            }
        }

        stage('Test') {
            steps {
                bat '.\\mvnw.cmd test'
            }
            post {
                always {
                    junit 'target\\surefire-reports\\*.xml'
                }
            }
        }

        stage('Run') {
            steps {
               script {
                   bat '''for %%f in (target\\*.jar) do "%JAVA_HOME%\\bin\\java" -Dserver.port=8090 -jar "%%f"'''
               } 
            }
        }
    }
}

pipeline {
    agent any
    environment {
        GIT_URL = 'https://github.com/Tope79/MyProject.git'
        TOMCAT_URL    = 'http://52.15.237.50:8080/'
    }
    stages {
        stage('Git Clone') {
            steps {
                git "${GIT_URL}"
            }
        }
        stage("SonarQube analysis") {
            steps {
              withSonarQubeEnv('sonar') {
                sh 'mvn ~f SampleWebApp/pom.xml clean package sonar:sonar'
              }
            }
          }
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-id',
                path: '',
                url: "${TOMCAT_URL}")],
                contextPath: 'mypipeApp',
                war: '**/*.war'
            }
        }
    }
}

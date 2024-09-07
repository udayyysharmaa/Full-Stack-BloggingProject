pipeline {
    agent any
    tools{
        maven 'maven3'
        jdk 'jdk17'
    }
    environment{
        SONAR_HOME = tool "sonar-scanner"
    }

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/udayyysharmaa/Full-Stack-BloggingProject.git'
            }
        }
        stage('Code Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Code Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Sonar-Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=nexus -Dsonar.projectKey=nexus -Dsonar.java.binaries=target'
                }
            }
        }
        stage('Deploy and Artifactes') {
            steps {
                withMaven(globalMavenSettingsConfig: '', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '1d0db06e-98fd-45a9-8ff5-da57ba3f9ecf', traceability: true) {
                    sh 'mvn deploy'
                }
            }
        }
    }
}

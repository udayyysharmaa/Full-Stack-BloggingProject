pipeline {
    agent any
    tools{
        jdk 'jdk17'
        maven 'maven'
    }
    environment {
        SONAR_HOME = tool "sonar-scanner"
    } 

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/udayyysharmaa/Full-Stack-BloggingProject.git'
            }
        }
        stage('Build Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Sonarqube-Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=Fullstack-projectforclient -Dsonar.projectKey=Fulltack-Projectforclient  -Dsonar.java.binaries=target'
                }
            }
            
        }
        stage('Sonar Quality Check') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage('Code Build') {
            steps {
                sh 'docker build -t websitehost:v2 .'
            }
        }
        stage('Trivy') {
            steps {
                sh 'trivy image websitehost:v2'
            }
        }
        stage("Push to Private Docker Hub Repo"){
            steps{
                withCredentials([usernamePassword(credentialsId:"DockerHubCred",passwordVariable:"dockerPass",usernameVariable:"dockerUser")]){
                sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                sh "docker tag  websitehost:v2 ${env.dockerUser}/websitehost:v2"
                sh "docker push ${env.dockerUser}/websitehost:v2"
                }
                
            }
        }
        stage('Code Deploy') {
            steps {
                sh 'docker run -d --name FullstackProject -p 8080:8080 websitehost:v2 '
            }
        }
    }
}

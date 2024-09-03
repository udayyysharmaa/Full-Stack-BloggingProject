pipeline { 
    agent any
    
    tools {
        maven 'maven'
        jdk 'jdk17'
    }

    stages {
        
        stage('Compile&Deploy') {
            steps {
            sh  "mvn compile"
            }
        }
        
        stage('Test&build') {
            steps {
                sh "mvn test"
            }
        }
        
        stage('Package') {
            steps {
                sh "mvn package"
            }
        }
    }
}

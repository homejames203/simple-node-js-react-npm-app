pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3000:3000' 
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('YaddaYadda') { 
            
            steps {
                def packageJSON = readJSON file: 'package.json'
            def packageJSONVersion = packageJSON.version
                echo packageJSONVersion
            }
            
            
        }
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}
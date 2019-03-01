@Library('jenkins-shared-scripts')_
import com.kineticdata.*
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
                script {
                    
                    def packageJSON = readJSON file: 'package.json'
                    def packageJSONVersion = packageJSON.version
                    def version = new SemVer(packageJSONVersion)
                    echo version.major
                    echo version.toString()
                    
                }
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
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
                    def semVerVersion = new SemVer(packageJSONVersion)
                    // Upload to S3
                    def major = "${semVerVersion.major.toString()}.x.x"
                    def minor = "${semVerVersion.major.toString()}.${semVerVersion.minor.toString()}.x"
                    def patch = "${semVerVersion.major.toString()}.${semVerVersion.minor.toString()}.${semVerVersion.patch.toString()}"
                    echo "${major}, ${minor}, ${patch}"
                    echo env.BRANCH_NAME
                    
                }
                echo env
            }
        }
        // stage('Build') { 
        //     steps {
        //         sh 'npm install' 
        //     }
        // }
        // stage('Test') {
        //     steps {
        //         sh './jenkins/scripts/test.sh'
        //     }
        // }
    }
}
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
        stage('Print Variables') {
            echo sh(returnStdout: true, script: 'env')
        }
        stage('Upload Release') { 
            when { tag "release-*" }
            steps {
                script {
                    echo "IS RELEASE"
                    def releaseLocation = '/releases'
                    def packageJSON = readJSON file: 'package.json'
                    def packageJSONVersion = packageJSON.version
                    def semVerVersion = new SemVer(packageJSONVersion)
                    // Upload to S3
                    def major = "${semVerVersion.major.toString()}.x.x"
                    echo "Uploading major to ${releaseLocation}/${major}"
                    def minor = "${semVerVersion.major.toString()}.${semVerVersion.minor.toString()}.x"
                    echo "Uploading minor to ${releaseLocation}/${minor}"
                    def patch = "${semVerVersion.major.toString()}.${semVerVersion.minor.toString()}.${semVerVersion.patch.toString()}"
                    echo "Uploading patch to ${releaseLocation}/${patch}"       
                }
            }
        }
        stage('Upload Branch') { 
            when { not { tag "release-*" } }
            steps {
                script {
                    echo "IS JUST BRANCH"
                    echo "Branch Name: ${env.BRANCH_NAME}" 
                }
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
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
        ROOT_BUCKET_LOCATION = 's3://kinops.io/bundles/'
        BUNDLE_NAME = sh(returnStdout: true, script: 'echo `expr "$GIT_URL" : \'^.*/request-ce-bundle-\\(.*\\)\\.git$\'`').trim()
    }
    stages {
        stage('Upload to S3') {
            steps {
                script {
                    if(env.TAG_NAME && env.TAG_NAME.startsWith("release-")) {
                        echo "IS RELEASE"
                        def releasesBucketLocation = "${env.BUNDLE_NAME}/releases"
                        def packageJSON = readJSON file: 'package.json'
                        def packageJSONVersion = packageJSON.version
                        def semVerVersion = new SemVer(packageJSONVersion)
                        def major = "${semVerVersion.major.toString()}.x.x"
                        def minor = "${semVerVersion.major.toString()}.${semVerVersion.minor.toString()}.x"
                        def patch = "${semVerVersion.major.toString()}.${semVerVersion.minor.toString()}.${semVerVersion.patch.toString()}"
                        [major,minor,patch].each { release ->
                            def uploadLocation = "${env.ROOT_BUCKET_LOCATION}/${releasesBucketLocation}/${release}"
                            echo "Uploading Release (${release}) to S3 ${uploadLocation}"
                        }
                    }
                    def branchesBucketLocation = "${env.BUNDLE_NAME}/branches"
                    def uploadLocation = "${env.ROOT_BUCKET_LOCATION}/${branchesBucketLocation}/${env.BRANCH_NAME}"
                    echo "Uploading Branch (${env.BRANCH_NAME}) to S3 ${uploadLocation}"
                    // OPTIONS = '--acl public-read --cache-control="must-revalidate, max-age: 0" --delete'
                    // sh "aws s3 sync packages/app/build s3://kinops.io/bundles/hydrogen/${BUNDLE}/${VERSION} ${OPTIONS}"
                }
            }  
        }
    }
}
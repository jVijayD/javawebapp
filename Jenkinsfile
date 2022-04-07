pipeline {
    agent any
   tools {
    maven 'Maven3.8.4'	
	}
         stages {
             stage('checkout') {
                 steps {
                 checkout([$class: 'GitSCM', branches: [[name: '*/master' , name: '*/test']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jVijayD/javawebapp.git']]])
            }
             }
             stage('Build') {
                 steps {
                     sh 'mvn clean install -f pom.xml'
                 }
             }
             stage('Deploy to s3 keyjen') {
                 when {
                     branch 'master'
                 }
                 steps {
                     s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'keyjen', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '**/*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'awsbucket', userMetadata: []
             }
        }
             stage('Deploy to S3 expojen ') {
             when {
                     branch 'test'
                 }
                 steps {
                     s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'expojen', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '**/*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'awsbucket', userMetadata: []
             }
        }
         }
}

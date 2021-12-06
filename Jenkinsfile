pipeline {
agent {
    node {
        label 'slave1'
}}
triggers {
    githubPush()    
    }
environment {
dotnet = '/usr/bin/dotnet.exe'
}
stages {
stage('Cleancache') {
      steps {
            sh 'rm -rf /home/ubuntu/jenkins/workspace/dotnet/*'
       }
    }
stage ('Checkout') {
            steps {
                //this is a comment 
                git credentialsId: 'userId', url: 'https://github.com/wonks2021/nettake1.git',branch: 'master'
            }
}
stage ('Restore PACKAGES') {     
         steps {
             sh "export PATH=/usr/bin/dotnet:$PATH"
             sh "dotnet restore"
          }
        }
stage('Clean') {
      steps {
            sh 'dotnet clean'
       }
    }
stage('Build') {
     steps {
            sh 'dotnet build'
      }
   }
 stage('Run') {
      steps {
            sh 'dotnet run'
      }
   }
stage('Pack') {
     steps {
           sh 'dotnet pack --no-build --output nupkgs'
      }
   }
    	stage ('Server'){
            steps {
               rtServer (
                 id: "Artifactory",
                 url: 'https://projectnuget.jfrog.io/artifactory',
                 username: 'admin1',
                  password: 'Admin@123',
                  bypassProxy: true,
                   timeout: 300
                        )
            }
        }
        stage('Upload'){
            steps{
                rtUpload (
                 serverId:"Artifactory" ,
                  spec: '''{
                   "files": [
                      {
                      "pattern": "*.nupkg",
                      "target": "nuget101-nuget-local"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage('Download'){
            steps{
                rtDownload (
                 serverId:"Artifactory" ,
                  spec: '''{
                   "files": [
                      {
                      "pattern": "nuget101-nuget-local/SampleCliApp.1.0.0.nupkg",
                      "target": "/home/ubuntu/jenkins/workspace/dotnet/"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Artifactory"
                )
            }
        }
 }
}

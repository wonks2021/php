pipeline {
agent {
    node {
        label 'slave1'
}}
triggers {
    githubPush()    
    }
stages {
stage('Cleancache') {
      steps {
            sh 'rm -rf /home/ubuntu/jenkins/workspace/php/*'
       }
    }
stage ('Checkout') {
            steps {
                //this is a comment 
                git credentialsId: 'userId', url: 'https://github.com/wonks2021/php.git',branch: 'main'
            }
}
    
stage ('Run Script') {
    steps { 
        sh "sudo zip sample.zip index.php"
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
                      "pattern": "path/*",
                      "target": "sample-php"
                      }
                            ]
                           }''',
                        )
            }
        }
 }
}

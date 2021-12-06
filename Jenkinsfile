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
        sh "sudo mv index.php /var/www/html/"
    } 
        }    
 }
}

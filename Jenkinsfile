pipeline {
    agent any
    tools{
         maven 'Maven'
    }
    stages{
         stage("SCM checkout"){ // we can give any name under stage 
             steps {
                 checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/begumraziya98/jenkins-ci-cd.git']])
             }
         }
         
           stage("Build Processe"){
             steps {
                 script{
                     bat 'mvn clean install'
                 }
             }
         }
         stage("Deploy to Container"){
             steps{
                 deploy adapters: [tomcat9(credentialsId: 'tomcat-pwd', path: '', url: 'http://localhost:9090/')], contextPath: 'jenkinsCiCd', war: '**/*.war'
             }
         }
    }
    
     post {
        always {
            emailext attachLog: true,
                     body: '''<html>
                            <body>
                                <p>Build Status: ${BUILD_STATUS}</p>
                                <p>Build Number: ${BUILD_NUMBER}</p>
                                <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
                            </body>
                            </html>''',
                     mimeType: 'text/html',
                     replyTo: 'begumraziya98@gmail.com',
                     subject: "Pipeline Status : ${BUILD_NUMBER}",
                     to: 'begumraziya98@gmail.com'
        }
    }
    //SCM checkout
    // build
    //deploy war
    // email
}
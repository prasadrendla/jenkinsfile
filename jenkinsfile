pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('checkout'){
            steps{
            git 'https://github.com/prasadrendla/javawebapp.git'
            }
        }
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('slack-build-notification') {
           steps{
               slackSend channel: '#projectx', message: 'build successfull', teamDomain: 'prasads-global', tokenCredentialId: 'slack'
           }
       }
       stage ('CodeQulity') {
            steps{
            withSonarQubeEnv('sonarqube') {
                 sh 'mvn clean install -f pom.xml sonar:sonar' 
                }
            }
        }
        stage('deploy-to-prod') {
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat service', path: '', url: 'http://52.5.168.39:8080')], contextPath: null, war: '**/*.war'
            }
       }
       stage('slack-deploy-notification') {
           steps{
               slackSend channel: '#projectx', message: 'deployed successfull', teamDomain: 'prasads-global', tokenCredentialId: 'slack'
           }
       }
       
    }   
}

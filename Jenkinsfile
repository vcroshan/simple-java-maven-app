pipeline{
    agent any
    tools{
        maven "Maven 3.8.4"
    }
    
    
    stages{
        stage("checkOut"){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Madhumitha611999/simple-java-maven-app.git']]])
            }
        }
        stage("Build"){
            steps{
                sh "mvn clean install" 
            }
        }
    }
}

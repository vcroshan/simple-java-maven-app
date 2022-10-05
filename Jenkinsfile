pipeline{
    agent any
    stages{
        stage("compile stage"){

            steps{
               withmaven(maven:"maven_3.8.6") {
                sh "maven clean compile"
               }
            }
        }
        stage ("Testing Stage"){
            steps{
                withmaven(maven:"maven_3.8.6") {
                sh "maven test"
               }
            }
        }
        stage ("Deployment Stage"){
            steps{
                withmaven(maven:"maven_3.8.6") {
                sh "maven Deploy"
               }
            }
       }
    }
}
pipeline{
    agent any 
    tools{
        maven 'mymaven'
    }
    stages{
        stage("build"){
            steps{
                script{
                    sh 'mvn install'
                }

            }
        }
        stage("unit testing"){
            steps{
                
                sh 'mvn test'
            }
            post{
                success{
                     echo "junit testing is success,publishing report"
                     junit 'target/surefire-reports/*.xml'
                
                }
                failure{
                    echo "junit testing is failed"

                }
            }
        }
        stage("sonar"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'Mysonar') {
                        sh "${tool("MySonarScaneer")}/bin/sonar-scanner \
                        -Dsonar.projectKey=java \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://172.31.13.146:9000 \
                        -Dsonar.login=sqp_160fde0e1530ac062cf75adbe45ca3d5edf8fc7e"
    
                    }
                }
            }

        }
        stage("upload artifact"){
            steps{
               sh 'mvn -s settings.xml deploy'
            }
        }
        
        

    }

}
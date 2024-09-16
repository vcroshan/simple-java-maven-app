pipeline{
    agent any
    tools {
  git 'Default'
  maven 'maven'
}

    stages{
        stage("checkout code"){
            steps{
                echo "========executing checkout code========"
                checkout([$class: 'GitSCM', branches: [[name: '*/vivek']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/gviveknath/simple-java-maven-app.git']]])
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========checkout code executed successfully========"
                }
                failure{
                    echo "========checkout code execution failed========"
                }
            }
        }
        stage("bulid"){
            steps{
                echo "========executing bulid========"
                sh 'mvn clean install'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========bulid executed successfully========"
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
                failure{
                    echo "========bulid code execution failed========"
                }
            }
        }
        stage("unit test"){
            steps{
                echo "========executing test========"
                sh 'mvn test'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========test executed successfully========"
                    junit 'target/surefire-reports/*.xml'
                }
                failure{
                    echo "========test code execution failed========"
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage("sonar scan"){
            steps{
                echo "========executing sonar scan========"
                withSonarQubeEnv('MysonarQube') {
                    sh "${tool('MySonarScaneer')}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple-java-maven-app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target/* \
                        -Dsonar.host.url=http://172.31.10.64:9000 \
                        -Dsonar.login=sqp_4cf0d67e258a6c9e26be1b843850595b29b5a749"
                }
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========test executed successfully========"
                    
                }
                failure{
                    echo "========test code execution failed========"
                    
                }
            }
        }
         stage("upload to nexus"){
            steps{
                echo "========uploading artifact========"
                sh 'mvn -s settings.xml deploy'
                
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========uploading successfully========"
                    
                }
                failure{
                    echo "========uploading failed========"
                    
                }
            }
        }
      }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
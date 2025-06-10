// Jenkinsfile

// Specify the shared library to use
// 'my-shared-library' should match the 'Name' configured in Jenkins Global Pipeline Libraries
// '@master' specifies the branch; change if you use 'main' or a specific version tag.
@Library('jenkins-shared-lib@main') _

pipeline {
    // You can use a specific agent or 'any'
    agent any

    environment {
        // Define environment variables used across stages
        MAVEN_TOOL_NAME = 'maven3' // Name of the Maven tool configured in Jenkins
        ARTIFACTORY_SERVER_ID = 'jfrog-artifactory' // ID of your Artifactory server in Jenkins
        ARTIFACTORY_REPO_KEY = 'libs-release-local' // Your target Artifactory repo
        SONARQUBE_SERVER_ID = 'SonarServer' // ID of your SonarQube server in Jenkins
        SONARQUBE_PROJECT_KEY = 'simple-java-maven-app' // Unique key for your project in SonarQube
        SONARQUBE_PROJECT_NAME = 'simple-java-maven-app' // Name for your project in SonarQube
        SOURCE_PATH = 'src/main/java'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Checking out source code..."
                    // Use a standard SCM checkout step (e.g., from GitHub)
                    // You might use 'checkout scm' or more specific git steps
                    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Roshan-Github', url: 'https://github.com/vcroshan/simple-java-maven-app.git']])
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building with Maven..."
                    // Call the shared library function for Maven build
                    // mavenBuild() // Uses default goal 'clean install'
                    mavenBuild(goal: 'clean install -DskipTests') // Example with custom goal and options
                }
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {
                    echo "Running SonarQube scan..."
                    // Call the shared library function for SonarQube scan
                    sonarScan(
                        serverName: env.SONARQUBE_SERVER_ID,
                        projectKey: env.SONARQUBE_PROJECT_KEY,
                        projectName: env.SONARQUBE_PROJECT_NAME,
                        projectVersion: commonUtils.generateBuildNumber(),
                        sourcePath: env.SOURCE_PATH
                    )
                }
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    echo "Publishing artifacts to Artifactory..."
                    // Call the shared library function to publish to JFrog Artifactory
                    jfrogPublish(
                        serverName: env.ARTIFACTORY_SERVER_ID,
                        repoKey: env.ARTIFACTORY_REPO_KEY,
                        buildInfoName: "${env.JOB_NAME}", // Or use a more specific name
                        buildNumber: commonUtils.generateBuildNumber()
                    )
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            // Add clean up or notification steps here
        }
        success {
            echo "Pipeline succeeded!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}

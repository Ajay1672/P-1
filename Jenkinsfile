pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Clone') {
            steps {
                git branch: 'main', // Ensure 'main' is the correct branch name
                    credentialsId: 'your-git-credentials-id', // Replace with your actual credentials ID
                    url: 'git@github.com:Ajay1672/new-Project-01-JAN-2024.git'
            }
        }

        stage('Clean') {
            steps {
                bat 'mvn clean' // Use 'sh' instead of 'bat' if running on Linux
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package' // Use 'sh' if running on Linux
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonumonu') { // Ensure 'sonumonu' is configured in Jenkins
                    bat """
                        mvn sonar:sonar ^
                          -Dsonar.projectKey=Docker-Jenkins-Java-Application ^
                          -Dsonar.host.url=http://localhost:9000
                    """
                }
            }
        }
    }
}

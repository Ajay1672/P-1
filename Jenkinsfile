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
                git branch: 'master', credentialsId: 'aj', url: 'git@github.com:Ajay1672/P-1.git'
            }
        }

        stage('Clean') {
            steps {
                bat 'mvn clean' // Use 'sh' instead of 'bat' if running on Linux
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package' // Use 'sh' instead of 'bat' if running on Linux
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

        stage('Upload to Nexus') {
            steps {
                script {
                    // Path to the generated artifact (JAR file)
                    def artifactPath = 'target/newProject-0.0.1-SNAPSHOT.jar'
                    
                    // Define the repository URL and credentials
                    def repositoryUrl = 'http://localhost:8081/repository/ajay-mvn/com/example/newProject/0.0.1/newProject-0.0.1-SNAPSHOT.jar'

                    if (fileExists(artifactPath)) {
                        echo "Uploading artifact to Nexus..."
                        bat """
                            curl -v -u admin:admin --upload-file ${artifactPath} ${repositoryUrl}
                        """
                    } else {
                        error "The artifact ${artifactPath} does not exist! Verify the build process."
                    }
                }
            }
        }
    }
}

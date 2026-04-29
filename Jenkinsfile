pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {

        stage("Build") {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('saidemy-sonarqube-server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

    }
}

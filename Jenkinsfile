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

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('saidemy-sonarqube-server') {
                    sh '''
                    mvn sonar:sonar \
                      -Dsonar.organization=saisaidemykey \
                      -Dsonar.projectKey=saisaidemykey_saidemytrend \
                      -Dsonar.projectName=saidemytrend
                    '''
                }
            }
        }

    }
}

pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {

        stage("Build") {
            steps {
                echo "----------- build started ----------"
                sh 'mvn clean package -DskipTests'
                echo "----------- build completed ----------"
            }
        }

        stage("Test") {
            steps {
                echo "----------- unit test started ----------"
                sh 'mvn test'
                echo "----------- unit test completed ----------"
            }
        }

        stage("SonarCloud Analysis") {
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

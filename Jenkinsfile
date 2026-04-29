// Define the URL of the Artifactory registry
def registry = 'https://trialu36tfr.jfrog.io/'

pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
        registry = "http://your-artifactory-url"   // 🔁 add your Artifactory URL
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

        stage("Artifact Publish") {
            steps {
                script {
                    echo '<--------------- Publish Started --------------->'

                    def server = Artifactory.newServer(
                        url: "${registry}/artifactory",
                        credentialsId: "artifact-cred"
                    )

                    def properties = "buildid=${env.BUILD_ID};commitid=${env.GIT_COMMIT}"

                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "webapp/target/*.war",
                                "target": "sai-libs-release-local/",
                                "props": "${properties}"
                            }
                        ]
                    }"""

                    def buildInfo = server.upload(uploadSpec)
                    server.publishBuildInfo(buildInfo)

                    echo '<--------------- Publish Ended --------------->'
                }
            }
        }

    }
}

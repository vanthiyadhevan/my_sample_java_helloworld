pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    def mavenHome = tool 'jenkins_maven'
                    sh "${mavenHome}/bin/mvn clean package"
                }
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    def server = Artifactory.newServer url: 'http://your-artifactory-url/artifactory', credentialsId: 'your-artifactory-credentials-id'
                    def buildInfo = Artifactory.newBuildInfo()

                    server.upload spec: '''{
                        "files": [
                            {
                                "pattern": 'target/*.jar',
                                "target": 'my_sample_java_helloworld/com/example/hello-world/${BUILD_NUMBER}/'
                            }
                        ]
                    }''', buildInfo: buildInfo
                }
            }
        }
    }

    post {
        success {
            echo 'Build and publish to Artifactory successful!'
        }
    }
}

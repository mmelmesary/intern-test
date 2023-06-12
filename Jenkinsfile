pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = 'instabugapp'
                    def dockerTag = 'v2.0'
                    def dockerRegistry = 'melmesary'

                    // Build Docker image
                    sh "docker build -t ${dockerRegistry}/${dockerImage}:${dockerTag} ."

                    // Check if build was successful
                    if (sh(returnStatus: true, script: "docker images ${dockerRegistry}/${dockerImage}:${dockerTag} | grep -q ${dockerTag}")) {
                        echo "Docker image built successfully"
                    } else {
                        error "Docker image build failed"
                    }

                    // Push image to Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                        sh "docker push ${dockerRegistry}/${dockerImage}:${dockerTag}"
                    }
                }
            }
        }
    }
}

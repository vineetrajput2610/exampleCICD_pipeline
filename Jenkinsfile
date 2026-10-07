pipeline {
    agent any
    environment {

        GITHUB_CONTAINER_REGISTRY = '<GITHUB_CONTAINER_REGISTRY_URL>'
        DOCKER_IMAGE = "$GITHUB_CONTAINER_REGISTRY/NAMESPACE/exampleJenkins"
        DOCKERFILE_PATH = 'Dockerfile'

        GITHUB_CONTAINER_REGISTRY_USER = credentials('githubcontainerregistryUser')
        GITHUB_CONTAINER_REGISTRY_PASSWORD = credentials('githubcontainerregistryPassword')

        CURRENT_BUILD_NUMBER = "${currentBuild.number}"
        GIT_COMMIT_SHORT = sh(returnStdout: true, script: "git rev-parse --short=15 ${GIT_COMMIT}").trim()
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER -f $DOCKERFILE_PATH .'
            }
        }
        stage('Push') {
            steps {
                sh 'docker login -u $GITHUB_CONTAINER_REGISTRY_USER -p $GITHUB_CONTAINER_REGISTRY_PASSWORD $GITHUB_CONTAINER_REGISTRY'
                sh 'docker push $DOCKER_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER'
            }
        }

    }

}
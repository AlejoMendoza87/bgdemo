#!groovy

pipeline {
    agent none
    options {
        timeout(time: 1, unit: 'DAYS')
        disableConcurrentBuilds()
    }
    stages {
        stage("Build and Register Image") {
            agent any
            steps { buildAndRegisterDockerImage() }
        }
    }
}

def initialize() {
    env.REGISTRY_URL = "http://10.130.2.201:8081"
    env.REGISTRY_CREDENTIALS = "nexus-qa"
    env.IMAGE_NAME = "bgdemo"
}

def buildAndRegisterDockerImage() {
    def buildResult
//  docker.withRegistry(env.REGISTRY_URL, env.REGISTRY_CREDENTIALS) {
    docker.withRegistry("10.130.2.201:8081", "nexus-qa") {
        echo "Build image"
//      buildResult = docker.build(env.IMAGE_NAME)
        buildResult = docker.build("bgdemo")
        echo "Register image at local registry"
        buildResult.push()
        echo "Disconnect from registry"
        sh "docker logout 10.130.2.201:8081"
    }
}

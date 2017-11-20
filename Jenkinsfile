node {

    checkout scm
    mvnHome = tool 'M3'


    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "patient-service"
    registryHost = "minikube/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "'${mvnHome}/bin/mvn' clean install"
        sh "docker build -t springio/gs-spring-boot-docker ."

    stage "Deploy"

        sh " kubectl apply -f deployment.yml"
        sh "kubectl rollout status deployment/patient-service"

}


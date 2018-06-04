node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"

    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-k8"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"

        sh "docker build -t ${imageName} -f applications/hello-k8/Dockerfile applications/hello-k8"

    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"

        sh "kubectl apply -f applications/hello-k8/k8s/deployment.yaml"
        sh "kubectl rollout status deployment/hello-k8"
}

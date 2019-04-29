node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"

        // kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'
        // don't want to have "manual-deployment.yaml" running after "deployment.yaml", as the later one overrides
        // deployment of image with name 127.0.0.1/hello-kenzan:$BUILD_TAG by image with name 127.0.0.1/hello-kenzan:latest

        kubernetesDeploy configs: "applications/${appName}/k8s/deployment.yaml", kubeconfigId: 'kenzan_kubeconfig'

}

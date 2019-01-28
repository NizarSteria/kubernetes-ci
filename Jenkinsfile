node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-nizar"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-nizar/Dockerfile applications/hello-nizar"
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "config"

        sh "kubectl config view"
        sh "kubectl config current-context"
        // sh "kubectl get service"
       

    stage "Deploy"

         kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'nizar_kubeconfig'
        // kubernetesDeploy configs: '/var/jenkins_home/.kube/config/applications/hello-nizar/k8s/deployment.yaml', kubeconfigId: 'nizar_kubeconfig'
         // sh "sed 's#__IMAGE__#'$BUILDIMG'#' applications/${appName}/k8s/deployment.yaml | kubectl apply -f -"

}
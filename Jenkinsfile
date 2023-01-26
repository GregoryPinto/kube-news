pipeline {
    agent any
    stages {
        stage ('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("mcgregorydouglas/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage ('Push docker image to docker hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockeruser') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage ('Deploy in Kubernetes'){
            steps{
                withKubeConfig([credentialsId: 'kubeconfig_grg']) {
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }

}
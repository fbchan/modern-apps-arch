pipeline {
    agent any
    environment
    {
        K8S_URL = 'https://rancher.foobz.com.au/k8s/clusters/c-wjr5k'
        CRED = 'kubeconfig_gke-2'
        NAMESPACE = 'default'
        TIMEOUT = '80s'
    }
    stages {
        stage('Deploy Juiceshop Apps') {
            steps {
                    withKubeConfig([credentialsId: CRED,
                    serverUrl: K8S_URL,
                    namespace: NAMESPACE
                    ]) {
                        sh 'kubectl apply -n sm-juiceshop -f sm-juiceshop-deploy.yml'
                        sh 'kubectl -n sm-juiceshop wait --for=condition=available --timeout=80s deployment/sm-juiceshop-deploy'
                        
                        }
            }
        }
        stage('Configure Application') {
            steps {
                    withKubeConfig([credentialsId: CRED,
                    serverUrl: K8S_URL,
                    namespace: NAMESPACE
                    ]) {
                        sh 'kubectl -n sm-juiceshop apply -f secureingress-app-developer.yml'
                        
                        }
            }
        }
    }
}

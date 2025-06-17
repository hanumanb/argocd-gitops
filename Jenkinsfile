pipeline {
  agent any
  environment {
    IMAGE = "nginx-web"
    K8S = "k8s"
    ARGO = "nginx-web"
    ARGO_SERVER = "argocd-server.local"
    ARGO_TOKEN = credentials('argocd-token')
  }
  stages {
    stage('Checkout') { steps { git url: 'https://github.com/hanumanb/argocd-gitops.git', branch: 'main' } }
    stage('Build & Push') {
      steps {
        script {
          docker.build("${IMAGE}:${env.BUILD_ID}").push()
        }
      }
    }
    stage('Update k8s') {
      steps {
        sh """
          sed -i "s|image:.*|image: ${IMAGE}:${BUILD_ID}|" ${K8S}/deployment.yaml
          git config user.email "ci@myorg"
          git config user.name "CI User"
          git add ${K8S}/deployment.yaml
          git commit -m "ci: bump image to ${BUILD_ID}"
          git push origin main
        """
      }
    }
    stage('ArgoCD Sync') {
      steps {
        sh """
          argocd login ${ARGO_SERVER} --auth-token ${ARGO_TOKEN} --insecure
          argocd app sync ${ARGO} --prune --self-heal
        """
      }
    }
  }
}
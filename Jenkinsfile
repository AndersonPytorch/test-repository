REGION = 'ap-northeast-2'
EKS_API = 'https://6918042C2B9B60669CFCE2B59402AF83.gr7.ap-northeast-2.eks.amazonaws.com'
EKS_CLUSTER_NAME='test-cluster'
EKS_NAMESPACE='default'
EKS_JENKINS_CREDENTIAL_ID='kubectl-deploy-credentials'
ECR_PATH = '998902534284.dkr.ecr.ap-northeast-2.amazonaws.com'
ECR_IMAGE = 'test-repository'

node {
    stage('Clone Repository'){
        checkout scm
    }

    stage('Docker Build'){
        // Docker Build and Push to ECR
        docker.withRegistry("https://${ECR_PATH}", "ecr:${REGION}:aws-credentials"){
            def image = docker.build("${ECR_PATH}/${ECR_IMAGE}:${env.BUILD_ID}", "--network=host --no-cache .")
        }
    }
    stage('Kubernetes'){
        withKubeConfig([credentialsId: "kubectl-deploy-credentials",
                        serverUrl: "${EKS_API}",
                        clusterName: "${EKS_CLUSTER_NAME}"]){

            sh "sed 's/IMAGE_VERSION/${env.BUILD_ID}/g' nginx-deployment.yaml > output.yaml"
            sh "aws eks --region ${REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}"
            sh "kubectl apply -f output.yaml"
        }
    }

}





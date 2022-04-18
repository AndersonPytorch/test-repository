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

    stage('Build to ECR'){
        // Docker Build and Push to ECR
        docker.withRegistry("https://", "ecr::aws-credentials"){
            image = docker.build("/", "--network=host --no-cache .")
        }
    }

}

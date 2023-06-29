/* groovylint-disable CompileStatic, NestedBlockDepth */
pipeline {
    agent any

    stages {
        stage('Pull SCM') {
            steps {
                // Checkout source code from repository
                checkout scm
            }
        }

        stage('Integrate Jenkins with EKS Cluster and Deploy App') {
            environment {
                EKS_CLUSTER_NAME = 'OE-DevOps-cluster'
                AWS_REGION = 'us-east-1'
                NAMESPACE = 'nginx'
            }
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    script {
                        sh('aws eks update-kubeconfig --name $EKS_CLUSTER_NAME --region $AWS_REGION')
                        sh "kubectl create namespaces $NAMESPACE"
                        sh "kubectl apply -f ci/deployment.yaml -f ci/service.yaml -n $NAMESPACE"
                    }
                }
            }
        }
    }
}

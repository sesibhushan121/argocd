pipeline {
    agent any
    
    stages {       
        /*stage('Prepare') {
            steps {
                checkout([$class: 'GitSCM',
                branches: [[name: "origin\main"]],
                doGenerateSubmoduleConfigurations: false,
                submoduleCfg: [],
                userRemoteConfigs: [[
                    url: 'ssh:\\git@git.example.com\argocd-test\argocd-test.git']]
                ])
            }
        }*/
        stage ('Docker_Build') {
            steps {
               
                    # Build the image
                sh'''
                    
                    
                          docker login -u sesibhushan121 -p sesi2020
                          docker build -f Dockerfile -t nginx:latest .
                          docker tag nginx:latest sesibhushan121/nginx:latest
                          docker push sesibhushan121/nginx:latest
                '''       
            }
        }
        
        /*    stage ('Deploy_K8S') {
             steps {
                     withCredentials([string(credentialsId: "jenkins-argocd-deploy", variable: 'ARGOCD_AUTH_TOKEN')]) {
                        sh '''
                        ARGOCD_SERVER="argocd-prod.example.com"
                        APP_NAME="debian-test-k8s"
                        CONTAINER="k8s-debian-test"
                        REGION="eu-west-1"
                        AWS_ACCOUNT="$ACCOUNT_NUMBER"
                        AWS_ENVIRONMENT="staging"

                        $(aws ecr get-login --region $REGION --profile $AWS_ENVIRONMENT --no-include-email)
                        
                        # Deploy image to ECR
                        docker tag $CONTAINER:latest $AWS_ACCOUNT.dkr.ecr.$REGION.amazonaws.com\$CONTAINER:latest
                        docker push $AWS_ACCOUNT.dkr.ecr.$REGION.amazonaws.com\$CONTAINER:latest
                        IMAGE_DIGEST=$(docker image inspect $AWS_ACCOUNT.dkr.ecr.$REGION.amazonaws.com\$CONTAINER:latest -f '{{join .RepoDigests ","}}')
                        # Customize image 
                        ARGOCD_SERVER=$ARGOCD_SERVER argocd --grpc-web app set $APP_NAME --kustomize-image $IMAGE_DIGEST
                        
                        # Deploy to ArgoCD
                        ARGOCD_SERVER=$ARGOCD_SERVER argocd --grpc-web app sync $APP_NAME --force
                        ARGOCD_SERVER=$ARGOCD_SERVER argocd --grpc-web app wait $APP_NAME --timeout 600
                        '''
               }
            }
        }*/
    }
}


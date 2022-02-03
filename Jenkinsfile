properties([pipelineTriggers([githubPush()])])

pipeline { 
    agent any
    podTemplate(cloud: 'kubernetes', containers: [containerTemplate(alwaysPullImage: true, args: '9999999', command: 'sleep', image: 'sesibhushan121/nginx', livenessProbe: containerLivenessProbe(execArgs: '', failureThreshold: 0, initialDelaySeconds: 0, periodSeconds: 0, successThreshold: 0, timeoutSeconds: 0), name: 'nginx', resourceLimitCpu: '', resourceLimitEphemeralStorage: '', resourceLimitMemory: '', resourceRequestCpu: '', resourceRequestEphemeralStorage: '', resourceRequestMemory: '')], name: 'nginx') {
    // some block
}
    stages {
         stage('Build Docker image') { 
             steps {
                    sh """
                     docker build -f Dockerfile -t nginx:latest .
                     docker tag nginx:latest sesibhushan121/nginx:latest
                    """
                 
                
            }
        }
       stage('Pushing images to Docker'){
            steps {
                container('docker') {
                        sh 'docker login -u sesibhushan121 -p sesi2020'
                        sh 'docker push sesibhushan121/nginx:latest'
                }
            }
        }
        stage('Deploy on ' ) {
            steps {
                
                sh '''
                kubectl apply -f Deployment/image-update.yaml 
                
                '''
                
                
            }
        }
    }

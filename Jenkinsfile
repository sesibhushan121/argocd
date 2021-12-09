def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
  containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:latest', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {
  node(label) {
    def myRepo = checkout scm
    def gitCommit = myRepo.GIT_COMMIT
    def gitBranch = myRepo.GIT_BRANCH
    def shortGitCommit = "${gitCommit[0..10]}"
 

    stage('Create Docker images') {
      container('docker') {
        withCredentials([[$class: 'UsernamePasswordMultiBinding',
                          credentialsId: 'dockerhub',
                          usernameVariable: 'DOCKER_HUB_USER',
                          passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
                          sh """
                          cd Deploy
                          docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}
                          docker build -f Dockerfile -t nginx:latest .
                          docker tag nginx:latest sesibhushan121/nginx:latest
                          docker push sesibhushan121/nginx:latest
                          """
        }
      }
    }
    stage('Run kubectl') {
      container('kubectl') {
        sh """
        cd Deploy
        kubectl apply -f .
        pwd
        ls
        """
      }
    }
    stage('Run helm') {
      container('helm') {
        sh "helm list"
      }
    }
  }
}

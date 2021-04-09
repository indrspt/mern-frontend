env.DOCKER_REGISTRY = 'indraraspati'
env.DOCKER_IMAGE_NAME = 'mern-frontend'
node('master') {
	stage('Mern frontend') {
      echo 'Mern frontend'
    }
    stage('Git Clone from Github') {
        git url: 'https://github.com/indrspt/mern-frontend.git'
    }
    stage('Build Docker Image') {
        sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
    }
    stage('Push Docker Image to Docker Hub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('Deploy To Kubernetes Cluster') {
        sh'''sed -i "20d" frontend-deploy.yml'''
        sh'''sed -i "19 a \'\\'        image: indraraspati/mern-frontend:${BUILD_NUMBER}" frontend-deploy.yml && sed -i "s/''//"  frontend-deploy.yml'''
        sh "kubectl apply -f  frontend-deploy.yml"
   }
    stage('Remove Docker Image in local') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage("Clean Workspace"){
        cleanWs()
    }
}

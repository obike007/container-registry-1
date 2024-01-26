pipeline {
  agent any

  environment {
    SERVICE_NAME       = "container-registry"
    ORGANIZATION_NAME  = "frankisinfotech"
    DOCKERHUB_USERNAME = "frankisinfotech"
    REGISTRY_TAG       = "${DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
  
  stages {

    stage ('Print ENVs') {
      steps {
        sh 'printenv'
      }
    }
    
    stage ('Build Image') {
      steps {
        withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
          sh 'docker build -t tagNam .'
          sh 'docker push TagName'
        }
      }
    }
  }
}

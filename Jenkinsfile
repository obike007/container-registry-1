pipeline {
  agent any

  environment {
    SERVICE_NAME       = "container-registry"
    ORGANIZATION_NAME  = "frankisinfotech"
    DOCKERHUB_USERNAME = "frankisinfotech"
    REGISTRY_TAG       = "${DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
    REPO_TAG           = "public.ecr.aws/g0b5g9q2"
    PRIVATE_REPO_TAG   = "765176032689.dkr.ecr.eu-west-1.amazonaws.com"
    APP_NAME           = "ch2images"
    PRIVATE_APP_NAME   = "cohortimages
    VERSION            = "${BUILD_ID}"
  }

  stages {

    stage ('Print ENVs') {
      steps {
        sh 'printenv'
      }
    }
    
    stage ('Build Image') {
      steps {
        withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
          sh 'docker build -t ${REGISTRY_TAG} .'
          sh 'docker push ${REGISTRY_TAG}'
        }
      }
    }

    stage ('Publish to Public ECR') {
      steps {
         withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
          sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) ${REPO_TAG}'
          sh 'docker build -t ${APP_NAME}:${VERSION} .'
          sh 'docker tag ${APP_NAME}:${VERSION} ${REPO_TAG}/${APP_NAME}:${VERSION}'
          sh 'docker push ${REPO_TAG}/${APP_NAME}:${VERSION}'
         }
       }
    }

        stage ('Publish to Private ECR') {
          steps {
             withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {
                sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region eu-west-1) ${PRIVATE_REPO_TAG}'
                sh 'docker build -t ${PRIVATE_APP_NAME}:${VERSION} .'
                sh 'docker tag ${PRIVATE_APP_NAME}:${VERSION} ${PRIVATE_REPO_TAG}/${PRIVATE_APP_NAME}:${VERSION}'
                sh 'docker push ${PRIVATE_REPO_TAG}/${PRIVATE_APP_NAME}:${VERSION}'
         }
       }
    }
    
    stage ('Delete Images') {
      steps {
        sh 'docker rmi -f $(docker images -qa)'
      }
    }
  }
}

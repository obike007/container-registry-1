pipeline {
  agent any

  stages {

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

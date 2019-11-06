pipeline {
  agent {
    node {
      label 'go'
    }
  }
   environment {
     DOCKERHUB_CREDENTIAL_ID = 'dockerhub-id'
   }
  stages {
    stage('checkout scm ') {
      steps {
        checkout(scm)
      }
    }
    stage('prepare') {
      steps {
        container('go') {
          sh 'go get github.com/cpuguy83/go-md2man'
          sh 'yum install distgen -y'
        }

      }
    }
    stage('base image build') {
      steps {
        container('go') {
          sh '''git submodule update --init
make build TARGET=centos7 VERSIONS=base'''
        }

      }
    }
    stage('push to dockerhub'){
      when {
        branch 'master'
      }
      steps{
        container('go'){
          withCredentials([usernamePassword(passwordVariable : 'DOCKER_PASSWORD' ,usernameVariable : 'DOCKER_USERNAME' ,credentialsId : "$DOCKERHUB_CREDENTIAL_ID" ,)]) {
            sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
          }
          sh 'docker tag kubespheredev/s2i-base-centos7 kubespheredev/s2i-base-centos7:2.1'
          sh 'docker push kubespheredev/s2i-base-centos7'
        }
      }
    }
    stage('update language image'){
      when{
        branch 'master'
      }
      steps{
         build job:'../s2i-java/master', wait: false
         build job:'../s2i-python/master', wait: false
         build job:'../s2i-nodejs/master', wait: false
      }
    }
  }
}

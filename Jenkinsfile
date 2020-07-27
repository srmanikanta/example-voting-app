pipeline {
  agent {
    node {
      label 'JENKINS'
    }
  }
  stages {
    stage('Build result') {
      steps {
        sh 'docker build -t dockersamples/result ./result'
      }
    }
    stage('Build vote') {
      steps {
        sh 'docker build -t dockersamples/vote ./vote'
      }
    }
    stage('Build worker') {
      steps {
        sh 'docker build -t dockersamples/worker ./worker'
      }
    }
    stage('Push result image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'dockerbuildbot-index.docker.io', url:'') {
          sh 'docker push dockersamples/result'
        }
      }
    }
    stage('Push vote image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'dockerbuildbot-index.docker.io', url:'') {
          sh 'docker push dockersamples/worker'
        }
      }
    }
    stage('Create vote namespace') { 
      when {
          branch 'master'
      }
      steps {
          sh 'kubectl create namespace vote'
      }
    }
     stage('Create Deployment and service objects') { 
      when {
          branch 'master'
      }
      steps {
          sh 'kubectl create -f k8s-specifications/'
      }
    }
  }
}

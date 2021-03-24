pipeline {

  environment {
    registry = "chetangautamm/repo"
    registryCredential = '58881f31-29bb-48a8-9da9-fc254654146d' 
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source Code') {
      steps {
        git 'https://github.com/chetangautamm/helm-k8s-deployment.git'
      }
    }
    
    
    stage('Deploying Opensips CNF') {
      steps {
        sshagent(['k8suser']) {
          script {
            try {
              sh "ssh k8suser@52.172.221.4 helm install opensips https://chetangautamm.github.io/helm/opensips-0.1.0.tgz"
              sh "ssh k8suser@52.172.221.4 helm install uas https://chetangautamm.github.io/helm/sipp-0.1.0.tgz"
              sh "ssh k8suser@52.172.221.4 helm install uac https://chetangautamm.github.io/helm/sipp-0.1.0.tgz"
            }catch(error){
              sh "ssh k8suser@52.172.221.4 helm install opensips https://chetangautamm.github.io/helm/opensips-0.1.0.tgz"
              sh "ssh k8suser@52.172.221.4 helm install uas https://chetangautamm.github.io/helm/sipp-0.1.0.tgz"
              sh "ssh k8suser@52.172.221.4 helm install uac https://chetangautamm.github.io/helm/sipp-0.1.0.tgz"
            } 
          }
        }              
      }
    }
    stage('Validating Opensips Using SIPp') {
      steps {
        sh "chmod +x configure.sh"
        sshagent(['k8suser']) {
          sh "scp -o StrictHostKeyChecking=no -q configure.sh k8suser@52.172.221.4:/home/k8suser"
          script {
            sh "sleep 10"
            sh "ssh k8suser@52.172.221.4 ./configure.sh"
          }
        }              
      }
    }
  }
}

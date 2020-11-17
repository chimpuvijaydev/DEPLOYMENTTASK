pipeline {
    agent any
  
    stages {
      stage('checkout') {
           steps {
             
                git branch: 'Production', url: 'https://github.com/chimpuvijaydev/Deployment.git'
             
          }
        }
      
      stage('helm deploy') {
             
            steps {
               sh "helm repo add stable https://charts.helm.sh/stable"
               sh "helm repo update"
               sh "helm install rabbitmq ."
               sh "helm install mysql ."
               sh "helm install redis ."
               
}  
}
} 

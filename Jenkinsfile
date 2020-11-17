pipeline {
    agent any
    environment
     {
        registry = "vijaydocker/springboot"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
     
    stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'reponame'
             
          }
        }
      stage('Build') { 
            steps {
                sh 'mvn clean package' 
            }
        }  
      stage('Image Build'){
           steps{
               script{
                     dockerImage = docker.build registry + ":$BUILD_NUMBER"
                 }
             }
         }
      stage('Deploy our image') {
          steps{
            script {
              docker.withRegistry( '', registryCredential ) {
              dockerImage.push()
            }
        }
            }
        }
      stage('helm deploy') {
             
            steps {
               sh "helm repo add stable https://charts.helm.sh/stable"
               sh "helm repo update"
               sh "helm install springboot helm ."
               sh "helm install --generate-name stable/prometheus-operator"
               sh "helm install -n elasticsearch stable/elasticsearch --namespace=logs --set client.replicas=1 --set master.replicas=1 --set cluster.env.MINIMUM_MASTER_NODES=1 --set cluster.env.RECOVER_AFTER_MASTER_NODES=1 --set cluster.env.EXPECTED_MASTER_NODES=1 --set data.replicas=1 --set data.heapSize=300m --set master.persistence.storageClass=elasticsearch-master --set master.persistence.size=5Gi --set data.persistence.storageClass=elasticsearch-data --set data.persistence.size=5Gi"
               sh "helm install -n fluent-bit stable/fluent-bit --namespace=logs --set backend.type=es --set backend.es.host=elasticsearch-client"
               sh "helm install -n kibana stable/kibana --namespace=logs --set env.ELASTICSEARCH_HOSTS=http://elasticsearch-client:9200 --set service.type=NodePort --set service.nodePort=31000" 

}
}  
}
} 

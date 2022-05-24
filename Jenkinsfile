node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALS', url:  'https://github.com/Kanikasharmaaa/DevopsProject.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t kanikasharma96/application1 .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          sh "docker login -u kanikasharma96 -p ${DOKCER_HUB_PASSWORD}"
        }
        sh 'docker push kanikasharma96/application1'
     }
     
     stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'SpringBootkubernetes.yml', 
         kubeconfigId: 'KUBERNATES_CONFIG',
         enableConfigSubstitution: true
        )
     }
	 
	  /**
      stage("Deploy To Kuberates Cluster"){
        sh 'kubectl apply -f SpringBootkubernetes.yml'
        sh 'kubectl autoscale deployment mydeploy --cpu-percent=20 --min=1 --max=10'
      } **/
     
}

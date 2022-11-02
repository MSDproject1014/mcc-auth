
node {
    stage ("Checkout AuthApi") {
        git branch: 'main', url: 'https://github.com/MSDproject1014/mcc-auth.git'
    }
    
    stage ("Gradle Build - AuthApi") {
        sh 'gradle clean build'
    }
    
    stage ("Gradle Bootjar-Package - AuthApi") {
        sh 'gradle bootjar'
    }
    
    stage ("Containerize the app-docker build - AuthApi") {
        sh 'docker build --rm -t mccauth:v1.0 .'
    }
    
    stage ("Inspect the docker image - AuthApi") {
        sh "docker images mccauth:v1.0"
        sh "docker inspect mccauth:v1.0"
    }
    
    stage ("Run Docker container instance - AuthApi") {
        sh "docker run -d --rm --name mccauth -p 8081:8081 mccauth:v1.0"
     }
    
    stage('User Acceptance Test - AuthApi') {
	
	  def response= input message: 'Is this build good to go?',
	   parameters: [choice(choices: 'Yes\nNo', 
	   description: '', name: 'Pass')]
	
        if(response=="Yes") {
            stage('Deploy to Kubenetes cluster - AuthApi') {
                sh "kubectl create deployment mccauth --image=mccauth:v1.0"
                
                //get the value of API_HOST from kubernetes services and set the env variable
                //sh "mccdataIP=$(kubectl get service/mccdata -o jsonpath={.spec.clusterIP})"

                //sh "kubectl set env deployment/mccauth API_HOST=$mccdataIP:8080"

                sh "kubectl set env deployment/event-auth API_HOST=\$(kubectl get service/event-data -o jsonpath='{.spec.clusterIP}'):8080"
                
                sh "kubectl expose deployment mccauth --type=LoadBalancer --port=8081"
            }
	    }
    }

    stage("Production Deployment View") {
        sh "kubectl get deployments"
        sh "kubectl get pods"
        sh "kubectl get services"
    }
  
}

node {
    stage ("Checkout AuthService"){
        git branch: 'main', url: 'https://github.com/foxwas/sept26-bah-mcc-auth.git'
    }
    
    stage ("Gradle Build - AuthService") {
        sh 'gradle clean build'
    }
    
    stage ("Gradle BootJar-Package - AuthService") {
        sh 'gradle bootJar'
    }
    
    stage('User Acceptance Test - AuthService') {
	
	  def response= input message: 'Is this build good to go?',
	   parameters: [choice(choices: 'Yes\nNo', 
	   description: '', name: 'Pass')]
	
	  if(response=="Yes") {
	  
	    stage('Release - AuthService') {
	      sh 'gradle build -x test'
	      sh 'echo AuthService is ready to release!'
	    }
	  }
    }
}

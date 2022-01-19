pipeline{
agent any
tools 
{
    maven 'maven'
}
triggers{
 pollSCM('* * * * *')
}

options{
    timestamps()
    buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '2', daysToKeepStr: '', numToKeepStr: '2'))
}

 stages{
   
   stage('CheckoutCode')
   {
     steps{
        git credentialsId: 'Gitcreds', url: 'https://github.com/vamsimowa83/maven-web-application.git'
     }
   }
   stage('Build')
   {
       steps{
	  sh "mvn clean package"
	 }
   }
   stage('Build Docker Image')
   {
       steps{
           
         sh 'docker build -t vamsimowa83/maven-web-application:1.0 .'
           
       }
   }
   stage('Push Docker Image')
	{
	   steps{
	       withCredentials([string(credentialsId: 'Dockerpwd', variable: 'Dockerpwd')]){
	           sh "docker login -u vamsimowa83 -p ${Dockerpwd}"
	       }
                sh 'docker push vamsimowa83/maven-web-application:1.0'
	   }
	}	
   stage('Push artifact to nexus')
    {
      steps
      {
      sh "mvn deploy"
      }
    }
   stage('Push to SonarQube')
    {
      steps
      {
      sh "mvn sonar:sonar"
      }
    }
 }
}
   

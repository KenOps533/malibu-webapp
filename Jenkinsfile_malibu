node{
  def mavenHome = tool name: 'maven3.8.6'
  stage('1clonecode'){
    git "https://github.com/pricy/malibu-web-app"
	//sh "git  clone https://github.com/pricy/tesla-web-app.git"
   }
   stage('2Test&Build'){    
	sh "${mavenHome}/bin/mvn clean package"
   }
   stage('3CodeQuality'){    
	sh "${mavenHome}/bin/mvn sonar:sonar"
   }
   stage('4UploadArtifacts'){    
	sh "${mavenHome}/bin/mvn deploy"
   }
  stage('5deploy2UAT'){
    sh "echo 'Deploying Code to UAT' "
    deploy adapters: [tomcat9(credentialsId: 'tomcatCredentials', path: '', url: 'http://3.232.241.11:8080/')], contextPath: null, war: 'target/*war'
   }
  stage('6approvalGate'){
      sh "echo 'Ready for Review' "
      timeout(time:2, unit:'MINUTES'){
      input message: "Application is ready for deployment, Please review and approve"
      }
  }
  stage('7deploytoProd'){
    sh "echo 'Deploying Code to Prod' "
    deploy adapters: [tomcat9(credentialsId: 'tomcatCredentials', path: '', url: 'http://3.232.241.11:8080/')], contextPath: null, war: 'target/*war'
  }
  stage('8emailNotification'){
    emailext body: '''Hi All,

Please see the deployment status.

Pricy''', recipientProviders: [buildUser(), developers(), brokenBuildSuspects()], subject: 'Deployment Status', to: 'wamuriithi@gmail.com'
  }
 }

pipeline{
   agent any  
   tools {
      maven 'maven3.9.6'
   }
   stages{
    stage('1.CloneCode'){
      steps{
         sh  "echo cloning the latest app version"
         git 'https://github.com/odion26/my-web-app'
      }
    }
    stage('2.mvnBuild'){
      steps{
       sh "echo validate, compile and perform UnitTesting"
       sh "echo UnitTesting must passed for artifacts to be created"
       sh "mvn clean package"
      }

    }
    stage('3.CodeQuality'){
      steps{
         //sh "mvn sonar:sonar"
         sh "echo CodeQualityAnalysis completed"          
      }

    }
    stage('4.UploadArtifacts'){
      steps{
          sh "mvn deploy" 
          sh "echo artifacts uploaded successfully"
          sh "echo I am now a Build and Release Engineer"
          sh "echo Build and release completed"         
      }
    }
    stage('5.Deploy2UAT'){
      steps{
        sh "echo Deployment is ready for the client review"
        sh "echo using deploy to container plugin" 
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://172.31.19.43:8088')], contextPath: null, war: 'target/*war'

      }
    }
    stage('6.ManualApproval'){
      steps{
       sh "echo Please Review and confirm within 5 days"
       timeout(time:5, unit:'DAYS'){
       input message: 'Application ready for deployment, Please review and approve'     
       }
 
      }
    }
    stage('7.Deploy2Prod'){
      steps{
        sh "echo appliction reviewed,approved and ready for the market"
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://172.31.19.43:8088')], contextPath: null, war: 'target/*war'

      }
    }
  }
}

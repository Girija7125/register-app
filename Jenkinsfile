pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/Girija7125/register-app'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
        stage("SonarQube Analysis"){
           steps{
               script{
                   withSonarQubeEnv(credentialsId: 'sonarqube_credential'){
                   sh "mvn sonar:sonar"
              }
           }
         }
      }
       stage("Quality Gate") {
  steps {
    script {
      // Use the SonarQube server configuration (defined in Jenkins global settings)
      withSonarQubeEnv('sonarqube-server') { // Replace with your SonarQube server name
        // Wait for Quality Gate and handle the result
        def qgResult = waitForQualityGate(
          abortPipeline: false // Optional: Continue even if Quality Gate fails
        )
        
        // Check the status explicitly (recommended)
        if (qgResult.status != 'OK') {
          error "Quality Gate failed: ${qgResult.status}"
        }
      }
    }
  }
}

    }
}
       
       




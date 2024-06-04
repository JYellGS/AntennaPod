pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'gradle assembleDebug'
            }
        }
        stage('Upload To AppSweep') {
            steps { 
          
                withCredentials([string(credentialsId: 'appsweep-api-key',
                                        variable: 'appsweep_key')]) {
                  withEnv(["APPSWEEP_API_KEY=$appsweep_key"]){ 
                    sh(script: "./gradlew uploadToAppSweepFreeDebug", 
                       returnStdout: true)
                
                }
              }
            }
          }
    }
}

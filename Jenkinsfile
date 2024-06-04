pipeline {
    agent { docker { image 'maven:3.9.7-eclipse-temurin-17-alpine' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
        stage('Upload To AppSweep') {
            steps { 
              dir(PROJECT_DIR) { 
                withCredentials([string(credentialsId: 'appsweep-api-key',
                                        variable: 'appsweep_key')]) {
                  withEnv(["APPSWEEP_API_KEY=$appsweep_key"]){ 
                    sh(script: "./gradlew uploadToAppSweepDebug", 
                       returnStdout: true)
                  }
                }
              }
            }
          }
    }
}

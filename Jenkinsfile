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
        stage('Download GS CLI') {
            steps {
                sh 'echo y | curl -sS https://platform.guardsquare.com/cli/install.sh'
                sh 'guardsquare --version'
            }
        }
        stage('Run GS CLI to report scan') {
            steps {
                sh 'guardsquare scan summary --wait-for static `cat ${{ github.workspace }}/app/build/guardsquare/appsweep/lastBuildID.txt` --format '{.High}' | export high_issue_count=$1
                sh 'echo $high_issue_count'
            }
        }
    }
}
//

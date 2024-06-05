pipeline {
    agent any
    environment {
        APPSWEEP_API_KEY= credentials(appsweep-api-key)
    }
    stages {
        stage('build') {
            steps {
                sh 'gradle assembleDebug'
            }
        }
        stage('Upload To AppSweep') {
            steps { 
                    sh './gradlew uploadToAppSweepFreeDebug'
            }
          }
        stage('Download GS CLI') {
            steps {
                sh 'echo y | curl -sS https://platform.guardsquare.com/cli/install.sh'
            }
        }
        stage('Appsweep upload with CLI') {
            steps {
                  withCredentials([string(credentialsId: 'appsweep-api-key',
                                          variable: 'appsweep_key')]) {
                  withEnv(['APPSWEEP_API_KEY=$appsweep_key']){ 
                    sh(script: "build_id=${guardsquare scan app-free-debug.apk --format '{{.ID}}'}", 
                       returnStdout: true)
                
                }
                                }
            }
        }
        stage('Run GS CLI to report scan') {
            steps {
                //sh 'guardsquare scan results --wait-for static `cat ${{ github.workspace }}/app/build/guardsquare/appsweep/lastBuildID.txt` --format \'{.High}\' | export high_issue_count=$1
                //sh 'echo $high_issue_count'
                sh 'test'
            }
        }
    }
}
//

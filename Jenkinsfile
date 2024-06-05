pipeline {
    agent any
    environment {
        APPSWEEP_API_KEY= credentials('appsweep-api-key')
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
                    sh 'build_id = /Users/jared.yellen/.jenkins/workspace/Testing_develop/app/build/guardsquare/appsweep/lastBuildID.txt'
            }
          }
        stage('Download GS CLI') {
            steps {
                sh 'echo y | curl -sS https://platform.guardsquare.com/cli/install.sh'
            }
        }

         stage('Run GS CLI to report scan First') {
            steps {
                sh 'guardsquare scan summary --wait-for static build_id --format \"{.High}\" | export high_issue_count=$1'
                sh 'echo $high_issue_count'
            }
        }
        stage('Appsweep upload with CLI') {
            steps {
                sh 'guardsquare scan app-free-debug.apk'
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

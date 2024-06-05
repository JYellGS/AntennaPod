
pipeline {
    agent any
    environment {
        APPSWEEP_API_KEY= credentials('appsweep-api-key')
        build_id = ""
        high_issues = 0
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
                //script {    
                //    env.build_id = readFile '/Users/jared.yellen/.jenkins/workspace/Testing_develop/app/build/guardsquare/appsweep/lastBuildID.txt'
               // }    
               // sh 'echo $(env.build_id)'
            }
          }
        stage('Download GS CLI') {
            steps {
                sh 'echo y | curl -sS https://platform.guardsquare.com/cli/install.sh'
            }
        }

         //stage('Run GS CLI to report scan First') {
         //   steps {
         //       sh 'guardsquare scan summary --wait-for static build_id --format \"{.High}\" | export high_issue_count=$1'
         //       sh 'echo $high_issue_count'
          //  }
        //}
        stage('Run GS CLI to report scan') {
            steps {
                sh 'echo $env.high_issues'
                sh 'guardsquare scan summary --wait-for static `cat /Users/jared.yellen/.jenkins/workspace/Testing_develop/app/build/guardsquare/appsweep/lastBuildID.txt` --format \"{{.High}}\" | $env.high_issues = $1'
                sh 'echo $env.high_issues'
                //sh 'echo $high_issue_count'
                //sh 'echo test'
            }
        }
    }
}
//huh 

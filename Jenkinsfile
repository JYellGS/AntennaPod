high_issues = 0
build_id_g = ""

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
                script {    
                    build_id_g = readFile '/Users/jared.yellen/.jenkins/workspace/Testing_develop/app/build/guardsquare/appsweep/lastBuildID.txt'
                }    
                sh "echo ${build_id_g}"
            }
          }
        stage('Download GS CLI') {
            steps {
                sh 'echo y | curl -sS https://platform.guardsquare.com/cli/install.sh'
            }
        }

        
        stage('Run GS CLI to report scan results') {
            steps {
                script {
                    sh(script: "guardsquare scan results --wait-for static ${build_id_g}", returnStdout: true).trim().toInteger() 
                    //sh "echo ${high_issues}"
                }
            }    
        }

        
        stage('Run GS CLI to report scan') {
            steps {
                script {
                    high_issues = sh(script: "guardsquare scan summary --wait-for static --format \"{{.High}}\" ${build_id_g}", returnStdout: true).trim().toInteger() 
                    sh "echo ${high_issues}"
                }
            }    
        }
        stage('running high issues test') {
            steps {
                sh "echo ${high_issues}"
                script {
                    if (high_issues > 7) {
                        sh 'false' 
                    }
                }
            }
        }
    }
}


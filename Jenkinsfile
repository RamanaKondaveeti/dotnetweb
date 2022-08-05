 def project_folder = "/var/lib/jenkins/workspace/dontnetappweb/hello-world-api/bin/Debug/netcoreapp2.0"
 def JOB_NAME = 'DotnetSample'
 def backup_folder = '/var/lib/jenkins/workspace/webbackups'
//def Console_Output_URL = "${JOB_URL}${BUILD_NUMBER}/console

pipeline {
agent any

     environment {
        def timestamp = sh(script: "echo `date +%d%m%Y%H%M`", returnStdout: true).trim()
        //def timestamp = sh(script: "echo `date +%s`", returnStdout: true).trim()
    }

     options {
        timestamps()
    }
    stages {  

        stage ("Clone Repository") {
                steps {
                   git branch: 'main', url: 'https://github.com/RamanaKondaveeti/dotnetweb.git'
                }
            }  
        stage('Prep') {
            steps {
                script {
                    GIT_BRANCH=sh(returnStdout: true, script: 'git symbolic-ref --short HEAD').trim()
                    currentBuild.setDisplayName("#${currentBuild.number} [" + GIT_BRANCH + "]")
                    sh "export GIT_BRANCH=$GIT_BRANCH"
                }
            }
        }  
        stage ('Building dll') {
            steps {
               sh "dotnet build HelloWorldConsoleApp1.sln"
            }           
        }
        stage ('Copy proj to backup') {
            steps {
                script {
                    echo "Copying project folder to backup folder"
                   sh "cp -r ${project_folder} ${backup_folder}/${JOB_NAME}${currentBuild.number}_$timestamp"
                    echo "Current timestamp :: $timestamp"
                }
            }
        }
        stage('Quality Analysis') {
            parallel {
                // run Sonar Scan and Integration tests in parallel.
                stage('Integration Test') {
                    steps {
                        echo 'Run integration tests here...'
                    }
                }
        stage ('Declarative Post Options') {
            steps {
                script {
                    echo 'Sending Post Build Notifications'
                }
            }  
            }
    }
}
    }
}

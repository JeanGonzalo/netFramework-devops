import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import java.net.URL
import java.net.URLEncoder

@NonCPS
def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}

pipeline {
  agent {label "net"}


  environment {
      // General Variables for Pipeline
      PROJECT_ROOT = 'project-test'
      SONAR_HOST_URL = 'http://localhost:9000'
      SONAR_AUTH_TOKEN = '6d04544a33272dddd889aef89ee658badc6009b2'

  }

  stages {

      stage('Checkout') {
        steps {
        // Get Github repo using Github credentials (previously added to Jenkins credentials)
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/JeanGonzalo/netFramework-devops.git']]])        }
      }
    
      stage("SonarQube - Static Code Analysis") {
            // when {
            //     expression { !enableDeploy() }
            // }
            steps {
                // docker.image('newtmitch/sonar-scanner').inside('-v /var/run/docker.sock:/var/run/docker.sock --entrypoint="" --net net_devops') {
                //         sh "--version"
                //         }

                script {
                        sh "uname -a"
                        
                        sh  " sonar-scanner \
                                -Dsonar.projectKey=test-baufest \
                                -Dsonar.sources=. \
                                -Dsonar.host.url=http://localhost:9000 \
                                -Dsonar.login=6d04544a33272dddd889aef89ee658badc6009b2"
                            //-Dsonar.projectVersion='${projectVersion}' ${pullRequestParams} \                
                }
            }
      }

      // stage("Build .net framework") {
      //       steps {
      //           script {
                    

      //                   powershell  " MsBuild.exe /t:Rebuild "
      //           }
      //       }
      // }
  }
}

// Function to validate that the message returned from SonarQube is ok
def qualityGateValidation(qg) {
  if (qg.status != 'OK') {
    return true
  }
  return false
}
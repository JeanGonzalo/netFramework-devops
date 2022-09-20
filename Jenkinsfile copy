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
      SONAR_AUTH_TOKEN = 'd1230bedb29dab2e4f25939dd692b456a82e50c4'

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
                script {
                    

                        powershell  " sonar-scanner -X -D sonar.host.url=${SONAR_HOST_URL} \
                              -D sonar.login=${SONAR_AUTH_TOKEN} \
                              -D sonar.projectKey=${PROJECT_ROOT} \
                              -D sonar.projectName=${PROJECT_ROOT} "
                            //-Dsonar.projectVersion='${projectVersion}' ${pullRequestParams} \
                    
                    // timeout(time: 3, unit: 'MINUTES') {
                    // // In case of SonarQube failure or direct timeout exceed, stop Pipeline
                    // waitForQualityGate abortPipeline: qualityGateValidation(waitForQualityGate())
                    //}
                }
            }
      }

      stage("Build .net framework") {
            steps {
                script {
                    

                        powershell  " MsBuild.exe /t:Rebuild "
                }
            }
      }
  }
}

// Function to validate that the message returned from SonarQube is ok
def qualityGateValidation(qg) {
  if (qg.status != 'OK') {
    return true
  }
  return false
}
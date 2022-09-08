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
                    def projectVersion = getShortCommitId()
                    def pullRequestParams = ""

                    docker.image('sonarsource/sonar-scanner-cli:4.4').inside('-u 0') {
                      withSonarQubeEnv('Sonar Qube Server') {

                        pwsh  " sonar-scanner -X -Dsonar.host.url=\$SONAR_HOST_URL \
                              -Dsonar.login=\$SONAR_AUTH_TOKEN \
                              -Dsonar.projectKey='\$PROJECT_ROOT' \
                              -Dsonar.projectName='\$PROJECT_ROOT' 
                              
                            "
                            //-Dsonar.projectVersion='${projectVersion}' ${pullRequestParams} \
                      }
                    }
                }
            }
      }

      
      // stage('scan') {
      //     environment {
      //       // Previously defined in the Jenkins "Global Tool Configuration"
      //       scannerHome = tool 'sonar-scanner'
      //     }
      //     steps {
      //       // "sonarqube" is the server configured in "Configure System"
      //       withSonarQubeEnv('sonarqube') {
      //         // Execute the SonarQube scanner with desired flags
      //         sh "${scannerHome}/bin/sonar-scanner \
      //                     -Dsonar.projectKey=Project-Test:Test \
      //                     -Dsonar.projectName=Project-Test \
      //                     -Dsonar.projectVersion=0.0.${BUILD_NUMBER} \
      //                     -Dsonar.host.url=http://mysonarqube:9000 \
      //                     -Dsonar.sources=. \
      //                     -Dsonar.login=admin \
      //                     -Dsonar.password=admin"
      //       }
      //       timeout(time: 3, unit: 'MINUTES') {
      //         // In case of SonarQube failure or direct timeout exceed, stop Pipeline
      //         waitForQualityGate abortPipeline: qualityGateValidation(waitForQualityGate())
      //       }
      //     }
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
pipeline {
  agent any

  options {
    disableConcurrentBuilds()
  }
  
  environment {
      NEXUS_HOST="damosoft.internal.com"
      NEXUS_IP="10.0.2.3"
      REPO_URL="https://github.com/h3llmo/dtech-commons"
      MAVEN_IMAGE="maven:3.8.3-openjdk-17"
      MAVEN_SETTINGS="/var/jenkins_home/settings.xml"
  }
  
 
  stages {

    stage ('checkout') {
      steps {
        checkout changelog: false, scm: scmGit(branches: [[name: '*/develop']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/h3llmo/dtech-commons']])
        sh "cp ${env.MAVEN_SETTINGS} ${WORKSPACE}/settings.xml"
      }
    }

    stage ('deploy lib') {
      agent {
        docker reuseNode: true, image: "${env.MAVEN_IMAGE}", args: "--add-host ${NEXUS_HOST}:${NEXUS_IP}"
      }
      steps {
        sh "mvn -f dtech-common-utils/pom.xml clean package deploy --settings ${WORKSPACE}/settings.xml"
        junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
      }
    }   
  }
}

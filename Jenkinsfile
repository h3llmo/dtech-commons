pipeline {
  agent any
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.8.3-openjdk-17'
          args '''-v /media/dtech/maven-settings:/opt/maven -w /opt/maven
--add-host damosoft.internal.com:10.0.2.3'''
        }

      }
      steps {
        sh 'mvn -f dtech-common-utils/pom.xml clean package deploy -s /opt/maven/settings.xml'
      }
    }

  }
}
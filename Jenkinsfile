pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''
        ./gradlew build -x test
        '''
        sh 'ls -lrt'
      }
    }
    stage('Test') {
      post {
        always {
          archiveArtifacts(artifacts: 'build/libs/**/*.jar', fingerprint: true)
          junit 'build/test-results/test/*.xml'

        }

      }
      steps {
        sh './gradlew test'
      }
    }
    stage('SonarQube') {
      steps {
        withSonarQubeEnv 'Cloud'
          def scannerHome = tool 'SonarScanner3'
          sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar.properties"
      }
    }
  }
}

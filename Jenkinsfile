pipeline {
  agent {label 'ubuntu_slave'}
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Hello') {
      when { changeRequest() }
      steps {

        echo " $GIT_BRANCH"
        echo "This is $CHANGE_ID"
        echo "this is $CHANGE_BRANCH"
        echo "There is $CHANGE_TARGET"
      }
    }
    stage("sonar quality check"){
            agent {
                docker {
                    image 'openjdk:11'
                    args '-e SONAR_USER_HOME=/home/remote_user/Jenkins/.sonar'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube --stacktrace --scan '
                    }
                }
            }
        }
  }
}
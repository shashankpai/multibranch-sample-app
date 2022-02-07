pipeline {
  agent {label 'ubuntu_slave'}
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Decorate Pull Request') {
      agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli'
                    args '-e GIT_BRANCH=$GIT_BRANCH -e CHANGE_ID=$CHANGE_ID -e CHANGE_BRANCH=$CHANGE_BRANCH -e CHANGE_TARGET=$CHANGE_TARGET'
                }
      }
      when { changeRequest() }
      steps {
        script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'echo $GIT_BRANCH && echo $CHANGE_ID'
                            sh '''sonar-scanner -X -Dsonar.pullrequest.key=${CHANGE_ID} -Dsonar.pullrequest.branch=${CHANGE_BRANCH} -Dsonar.pullrequest.base=${CHANGE_TARGET} -Dsonar.analysis.mode=publish
                               '''
                    }

        echo "Main branch is $GIT_BRANCH"
        echo "This is $CHANGE_ID"
        echo "this is $CHANGE_BRANCH"
        echo "There is $CHANGE_TARGET"
      }
    }
    }
    // stage("sonar quality check"){
    //         agent {
    //             docker {
    //                 image 'openjdk:11'
    //                 args '-e SONAR_USER_HOME=/home/remote_user/Jenkins/.sonar'
    //             }
    //         }
    //         steps{
    //             script{
    //                 withSonarQubeEnv(credentialsId: 'sonar-token') {
    //                         sh 'chmod +x gradlew'
    //                         sh './gradlew sonarqube --stacktrace --scan '
    //                 }
    //             }
    //         }
    //     }
  }
}

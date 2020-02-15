pipeline {
  agent any
  environment {
    BUILD_WORK_PATH="$JENKINS_HOME/BUILD_TMP/$GIT_BRANCH/$BUILD_NUMBER/test-jenkins"
    TEST_RESULT_FILES="reports/**"
  }
  stages {
    stage('Call DownStream') {
      steps {
        build job: "../test-jenkins/$GIT_BRANCH", wait: true, propagate: true
      }
    }
  }

  post {
    always {
        echo "pipeline2 finished!"
    }
    success {
      echo "pipeline2 success!"
    }
    unstable {
      echo "pipeline2 unstable!"
    }
    failure {
      echo "pipeline2 failed!"
    }
  }
}

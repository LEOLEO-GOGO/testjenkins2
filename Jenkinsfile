pipeline {
  agent any
  environment {
    BUILD_WORK_PATH="$JENKINS_HOME/BUILD_TMP/$GIT_BRANCH/$BUILD_NUMBER/test-jenkins"
    TEST_RESULT_FILES="reports/**"
  }
  stages {
    stage('Call DownStream') {
      steps {
        script{
          //调用方法得到日志 并 输出
          def changeString = getChangeString()
          echo "changes: $changeString"
        }
        build job: "../test-jenkins/$GIT_BRANCH", parameters: [
                  booleanParam(name: 'triggeredByUpstream', value: true)
                ], wait: true, propagate: true
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

@NonCPS
def getChangeString() {
  MAX_MSG_LEN = 100
  def changeString = ""

  echo "Gathering SCM changes"
  def changeLogSets = currentBuild.changeSets
  for (int i = 0; i < changeLogSets.size(); i++) {
      def entries = changeLogSets[i].items
      for (int j = 0; j < entries.length; j++) {
        def entry = entries[j]
        affectedPaths = entry.affectedPaths
        // for (int k = 0; k < affectedPaths.length; k++) {
        //   affectedPath = affectedPaths[k]
          
        // }
        changeString += " - ${affectedPaths} [${entry.author}]\n"
      }
  }
  if (!changeString) {
      changeString = " - No new changes"
  }
  return changeString
}
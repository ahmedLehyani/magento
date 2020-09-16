node("node-name") {
  try {
    stage("Process") {
      error("This will fail")
    }
  } catch(Exception error) {
    currentBuild.result = 'SUCCESS'
    return
  }
  stage("Skipped") {
     // This stage will never run
  }
}

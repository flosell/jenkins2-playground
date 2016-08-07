stage name: 'build and test'

node {
  git url: 'https://github.com/jglick/simple-maven-project-with-tests.git'
  def mvnHome = tool 'M3'
  sh "${mvnHome}/bin/mvn -B -Dmaven.test.failure.ignore verify"
  step([$class: 'ArtifactArchiver', artifacts: '**/target/*.jar', fingerprint: true])
  step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
}


stage name: 'ci and qa'
parallel(ci: {
  node {
    echo 'deploying to ci'
    sh 'sleep 10'
  }
},qa: {
  node {
    echo 'deploying to qa'
    sh 'sleep 20'
  }
})

stage name: 'prod'
input: "Ready for Production?"
node {
  echo 'deploying to prod'
  sh 'sleep 10'
}

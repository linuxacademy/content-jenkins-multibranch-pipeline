pipeline{
  agent any
  environment{
    MAJOR_VERSION = 1
  }
  stages{
    stage('compile') {
      steps{
        sh 'javac -d . src/*.java'
        sh 'echo Main-Class: Rectangulator > MANIFEST.MF'
        sh 'jar -cvmf MANIFEST.MF rectangle.jar *.class'
      }
    }
    stage('run') {
      steps{
        sh 'java -jar rectangle.jar 3 4'
      }
    }
  }
  post{
    success{
      archiveArtifacts artifacts: 'rectangle.jar', fingerprint: true
      emailext(
        subject: 'Build Success ${BUILD_NUMBER}',
        body: 'More information of Build ${BUILD_NUMBER}: \n at ${BUILD_URL}',
        to: 'praveen.kumar@avizva.com'
      )
    }
  }
}

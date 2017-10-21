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
        sh 'jar -cvmf MANIFEST.MF rectangle_${MAJOR_VERSION}.${BUILD_NUMBER}.jar *.class'
      }
    }
    stage('run') {
      steps{
        sh 'java -jar rectangle_${MAJOR_VERSION}.${BUILD_NUMBER}.jar 3 4'
      }
    }
    stage('code promotion'){
      when{
        branch 'development'
      }
      steps{
        echo 'Stashing git changes'
        sh 'git stash'
        echo 'Checkout development branch'
        sh 'git checkout development'
        echo 'Pull the changes'
        sh 'git pull origin'
        echo 'Checkout master branch'
        sh 'git checkout master'
        echo 'Pull latest changes master branch'
        sh 'git config user.email praveen.sbb@gmail.com'
        sh 'git config user.name prvnkmr484'
        sh 'git pull origin'
        echo 'merging the changes'
        sh 'git merge development'
        echo 'Push the changes to master'
        sh 'git push origin master'
      }
    }
    stage('Git Tagging'){
      steps{
        echo 'Tagging the release'
        sh 'git tag rectangle_${MAJOR_VERSION}.${BUILD_NUMBER}'
        sh 'git push origin rectangle_${MAJOR_VERSION}.${BUILD_NUMBER}'
      }
    }
  }
  post{
    success{
      archiveArtifacts artifacts: "rectangle_${MAJOR_VERSION}.${BUILD_NUMBER}.jar", fingerprint: true
      emailext(
        subject: 'Build Success ${BUILD_NUMBER}',
        body: 'More information of Build ${BUILD_NUMBER}: \n at ${BUILD_URL}',
        to: 'praveen.kumar@avizva.com'
      )
    }
  }
}

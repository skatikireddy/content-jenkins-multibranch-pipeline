pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        sh 'javac -d . src/*.java'
        sh 'echo Main-Class: Rectangulator > MANIFEST.MF'
        sh 'jar -cvmf MANIFEST.MF rectangle.jar *.class'
      }
      post {
    success {
      archiveArtifacts artifacts: 'rectangle.jar', fingerprint: true
    }
  }
    }
    stage('run') {
      steps {
        sh 'java -jar rectangle.jar 7 9'
      }
    }
    stage('Promote development to master') {
    when {
      branch 'development'
    }
    steps {
      echo "Stashing local changes"
      sh 'git stash'
      echo "checking out Development"
      sh 'git checkout development'
      sh 'git pull origin'
      echo "Checking out Master"
      sh 'git checkout master'
      echo "Merging development into master"
      sh 'git merge development'
      echo "Git push to Origin"
      sh 'git push origin master'

    }
    }
  }
  
}

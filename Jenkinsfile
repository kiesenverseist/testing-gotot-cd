pipeline {
  agent any
  stages {
    stage('Pre-Build') {
      steps {
        sh '''if [ -d "builds" ]; then
  rm -rf builds
fi'''
        sh '''mkdir builds
mkdir builds/linux
mkdir builds/windows
mkdir builds/mac'''
        sh 'ls'
      }
    }
    stage('Build') {
      parallel {
        stage('Windows') {
          steps {
            sh 'godot --export "Windows Desktop" builds/windows/test.exe'
            sh 'zip -rj builds/Test-$BUILD_NUMBER-Windows.zip builds/windows/*'
          }
        }
        stage('Linux') {
          steps {
            sh 'godot --export \'Linux/X11\' builds/linux/test'
            sh 'zip -rj builds/Test-$BUILD_NUMBER-Linux.zip builds/linux/*'
          }
        }
        stage('Mac') {
          steps {
            sh 'godot --export \'Mac OSX\' builds/mac/test.app'
            sh 'mv builds/mac/* builds/Test-$BUILD_NUMBER-Mac.zip '
          }
        }
      }
    }
    stage('Post-Build') {
      steps {
        archiveArtifacts 'builds/*.zip'
      }
    }
  }
}
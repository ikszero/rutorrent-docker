pipeline {
  environment {
    registry = "romancin/rutorrent"
    registryCredential = 'dockerhub'
  }
  agent any
  stages {
    stage('Cloning Git Repository') {
      steps {
        git url: 'https://github.com/romancin/rutorrent-docker.git',
            branch: 'master'
      }
    }
    stage('Building image and pushing it to the registry') {
            steps {
                script {
                    def gitbranch = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                    def version = readFile('VERSION')
                    def versions = version.split('\\.')
                  
                    def major = '0.9.8-' + versions[0]
                    def minor = '0.9.8-' + versions[0] + '.' + versions[1]
                    def patch = '0.9.8-' + version.trim()

                    docker.withRegistry('', registryCredential) {
                        def image = docker.build registry + ":" + gitbranch
                        image.push()
                        image.push(major)
                        image.push(minor)
                        image.push(patch)
                    }
                }
            }
    }
 }
 post {
        success {
            telegramSend '[Jenkins] - Pipeline CI-rutorrent-docker $BUILD_URL finalizado con estado :: $BUILD_STATUS'    
        }
    }
}

node {
  def dockerHubRepo = 'aveesave@gmail.com/tilek365'
  def dockerHubCredentialsId = 'DockerJenkins'

  stage("Clone the project") {
    git branch: 'main', url: 'https://github.com/tilek-stars/restapidemo2.git'
  }

  stage("Compilation") {
    sh "chmod +x ./gradlew"
    sh "./gradlew clean build -x test"
  }

  stage("Tests and Deployment") {
    stage("Running unit tests") {
      sh "chmod +x ./gradlew"
      sh "./gradlew test"
    }
    stage("Deployment") {
      sh "chmod +x ./gradlew"
      sh 'nohup ./gradlew bootRun -Dserver.port=8080 &'
      sh 'echo 17082000Tilek | sudo docker login -u aveesave@gmail.com --password-stdin'
      sh 'sudo docker build -t tilek365/restapidemo:1.0 .'
    }
    stage('Push Docker Image') {
        script {
            docker.withRegistry('https://index.docker.io/v1/', dockerHubCredentialsId) {
                def image = docker.image("tilek365/restapidemo:1.0")
                image.push()
                image.push('1.0')
            }
        }
    }
  }
}

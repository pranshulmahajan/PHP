pipeline{
  agent any
  environment{
	imagename = "dikshant1994/php"
	dockerImage = ''
  }
  stages{
    stage("Cloning Git"){
      steps{
        echo 'Cloning Repo...'
		git([url: 'https://github.com/dikshantSharma1994/php.git', branch: 'master', credentialsId: 'GITHUB_CREDS'])
      }
    }
    stage('Building image') {
		steps{
			script {
				dockerImage = docker.build imagename + ":$BUILD_NUMBER"
			}
		}
	}
	stage('Deploy Image') {
		steps{
			script {
				docker.withRegistry( '', 'DockerHub' ) {
					dockerImage.push()
				}
			}
		}
	}
	stage('Archive artifacts, pull image and containerize') {
      steps{
        archiveArtifacts artifacts: 'template/template.php', onlyIfSuccessful: true
	sh "docker pull $imagename:$BUILD_NUMBER"
	sh "docker container run -d $imagename:$BUILD_NUMBER"
	sh "docker ps"
      }
    }
}
}

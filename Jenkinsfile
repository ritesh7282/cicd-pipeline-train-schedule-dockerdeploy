pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
      
  		stage('Building Images'){
			when {
				branch 'master'
			}
			
			steps{
				script {
					app=docker.build("ritesh72823912/train-schedule")
					app.inside{
						sh 'echo $(curl localhost:8080)'
				}
			}
		}
		
		stage('Push Docker image'){
			when {
				branch 'master'
			}
			
			steps{
				script {
					docker.withRegistry('https://hub.docker.com/','docker_hub'){
						app.push("${env.BUILD_NUMBER}")
						app.push("latest")
					}
				}
			}
		}
    }
}

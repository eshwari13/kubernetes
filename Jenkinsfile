pipeline{
	agent any
	stages{
		stage('Checkout'){
			steps{
				git branch: "main", url: 'https://github.com/eshwari13/kubernetes.git'
			
			}
			
		}
		
		stage('Build'){
			steps{
				sh 'chmod a+x mvnw'
				sh './mvnw clean package -DskipTests=true' 
			}
			
			post{
				always{
					archiveArtifacts 'target/*.jar'
				}
			}
		}
      stage('DockerBuild') {
            steps {
                sh 'docker build -t eshwari13/g3-allergy-service:latest .'
            }
        }
        stage('Login') {

			steps {
				sh 'echo eshwari13 | docker login -u eshwari13 --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push eshwari13/g3-allergy-service'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}
   }


pipeline {
    agent any
	tools {
	    maven "3.6.3"
	 	}
	stages {
	    stage('Git CheckOut') {
		    steps {
			   git branch: 'main', url: 'https://github.com/prathmeshrajmane/jfrog-pipeline.git'
			}
		}
        stage('Clean and Install') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('Package'){
            steps {
                sh 'mvn package'
             }
        }
	stage ('Server'){
            steps {
               rtServer (
                 id: "Artifactory",
                 url: 'http://192.168.1.103:8082/artifactory',
                 credentialsId: 'ccrreeddeennttiiaall' ,
                   bypassProxy: true,
                   timeout: 300
                        )
            }
        }
        stage('Upload'){
            steps{
                rtUpload (
                 serverId:"Artifactory" ,
                  spec: '''{
                   "files": [
                      {
                      "pattern": "*.war",
                      "target": "prathmesh-devops-lab-libs-snapshot-local"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Artifactory"
                )
            }
        }
    }
}

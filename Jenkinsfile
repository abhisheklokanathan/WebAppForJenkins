pipeline {
    agent any
	tools {
	    maven "maven-3.8.4"
	 	}
	stages {
	    stage('Git CheckOut') {
		    steps {
			  git branch: 'main', credentialsId: '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', url: 'https://github.com/abhisheklokanathan/WebAppForJenkins.git'
			}
		}
        stage('Clean and Install') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage ('Package'){
            steps {
                bat 'mvn package'
             }
        }
	stage ('Server'){
            steps {
               rtServer (
                 id: "Artifactory",
                 url: 'http://localhost:8082/artifactory',
                 username: 'admin',
                  password: 'Digital@10',
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
                      "target": "logic-ops-lab-libs-snapshot-local"
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

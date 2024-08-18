pipeline {
    agent {
        label 'node1' // Or 'any', 'none', 'docker', etc.
    }
    stages {
            stage('SonarQube Code Scan') {
        environment {
            scannerHome = tool 'sonar-scanner'
          }
            steps {
            withSonarQubeEnv('sonarserver') {
            sh "${scannerHome}/bin/sonar-scanner"
            }
        }  
      }
       
       stage('Quality Gate'){
            steps{
                sh """
	if [ "\$(curl  -s -u admin:P@ssw0rd -X GET 'http://172.27.17.48:9000/api/qualitygates/project_status?projectKey=Demophp' | jq -r '.projectStatus.status')" = "OK" ]; then
  	echo -e 'this passed quality gate validations'
 	 exit 0
	fi

	if [ "\$(curl -s -u admin:P@ssw0rd -X GET 'http://172.27.17.48:9000/api/qualitygates/project_status?projectKey=Demophp' | jq -r '.projectStatus.status')" = "ERROR" ]; then
 	 echo -e 'this failed quality gate validations'
 	 exit 1
	fi
      """
            }
        }
     }
}

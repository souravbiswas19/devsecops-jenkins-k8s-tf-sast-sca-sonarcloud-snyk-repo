pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   environment {
        SONAR_PROJECT_KEY = 'souravbuggywebapp'
        SONAR_ORGANIZATION = 'souravbuggywebapp'
        SONAR_HOST_URL = 'https://sonarcloud.io'
    }
   stages{
    stage('SonarCloud Analysis') {
            steps {
                echo 'Starting SonarCloud analysis...'
        
                withCredentials([
                    string(
                        credentialsId: 'sonarcloud-token',
                        variable: 'SONAR_TOKEN'
                    )
                ]) {
                    sh '''
                        mvn org.sonarsource.scanner.maven:sonar-maven-plugin:5.8.0.7211:sonar \
                        -Dsonar.projectKey=souravbuggywebapp \
                        -Dsonar.organization=souravbuggywebapp \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.token="$SONAR_TOKEN" \
                        -Dsonar.nodejs.executable=/usr/bin/node-22
                    '''
                }
            }
        }
	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }		
  }
}

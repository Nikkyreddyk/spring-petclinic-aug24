pipeline {
    agent any 
    tools {
        maven 'MAVEN_3.9.8' 
    }
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('SCM') {
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic.git', 
                    branch: 'main'
            }
            
        }
        stage('BUILD') {
            steps {
                withSonarQubeEnv(credentialsId: 'SONARCLOUD_TOKEN', installationName: 'SONAR_CLOUD') { // You can override the credential to be used
				    sh 'mvn clean package org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar -D sonar.organization=aug24 -D sonar.projectKey=177530443246fbe3ff69a506d75ee36ae68286a2'
			    }
			    junit testResults: '**/surefire-reports/*.xml'
			    archive '**/target/spring-petclinic-*.jar'

            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
	}
    }
}
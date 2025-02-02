pipeline{
    agent any
    options{
        timeout(time : 1, unit: 'HOURS')
    }
    triggers{
        pollSCM (' * * * * * ')
    }
    tools{
        maven 'MAVEN_3.9.9'
    }

stages{
    stage('SCM'){
        steps{
            git url:'https://github.com/Nikkyreddyk/spring-petclinic-aug24.git',
            branch:'main'

        }  
    }
    stage('BUILD') {
            steps {
                withSonarQubeEnv(credentialsId: 'SONARCLOUD_TOKEN', installationName: 'SONARCLOUD') { // You can override the credential to be used
				    sh 'mvn clean package org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar -D sonar.organization=nikkyreddyk -D sonar.projectKey=6af5c533f706f2c0775ab5d17a91cd10a9c3f099'
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
pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
        stage('git checkout') {
            steps {
                 git branch: 'main', changelog: false, poll: false, url: 'https://github.com/krunal-hande/project_2.git'
            }
        }
        stage('CODE COMPILE') {
			steps {
				sh "mvn clean compile"
			}
		}
		stage('SONARQUBE ANALYSIS') {
			steps {
				withSonarQubeEnv('sonar-scanner') {
			    	sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Devops-CICD \
				    -Dsonar.java.binaries=. \
				    -Dsonar.projectKey=Devops-CICD '''
				}  
			}
		}
		stage('BUILDING WEB') {
			steps {
				sh '''docker stop web1
                docker rm web1
                docker build -t website .
                docker run --name web1 -d -p 8081:80 website '''
				}  
		    }
		stage('mail setup') {
		    steps {
		        mail bcc: '', body: 'your build is successful....enjoyy!!!!!! ', cc: '', from: '', replyTo: '', subject: 'build successful', to: 'krunalhande409@gmail.com'
		    }
		}	
		}
    }



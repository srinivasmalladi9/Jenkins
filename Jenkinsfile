pipeline {
    agent any

    triggers {
        githubPush()
    }
    environment {
        GITHUB_REPO_URL = 'https://github.com/srinivasmalladi9/mindcircuit15d.git'
        GIT_HUB_BRANCH = 'main'
        CONTEXT_PATH = 'app'
        WAR_FILE_PATH = '/var/lib/jenkins/workspace/cicd-test/target/mindcircuitbatch15d-1.0.0.war'
        TOMCAT_URL = 'http://54.234.187.139:8082/'
        NEXUS_URL = 'http://54.91.177.63:8081/'
        
    }
    
    stages {
        stage('Clean WorkSpace') {
            steps {
                echo 'cleaning workspace....'
                cleanWs()
                echo 'cleaning cleanup completed'
            }  
        } 
        stage('Continuous Integrtaion') {
            steps {
                echo 'Continuous Integration going on ....'
                git branch: "${GIT_HUB_BRANCH}", url: "${GITHUB_REPO_URL}"
                echo 'Continuous Integration completed'
            }
        }
        stage('Continuous Build') {
            steps {
                echo 'Continuous Build going on...'
                sh 'mvn clean install'
                echo 'Continuous Build Completed'
            }
        }
        stage('Sonar Analysis'){
            steps {
                echo 'Sonar Analysis is Going on'
                withSonarQubeEnv(credentialsId: 'SonarCreds', installationName: 'sonarscanner') {
				mvn sonar:sonar \
				  -Dsonar.projectKey=devops \
                  -Dsonar.host.url=http://54.87.128.170:9000 \
                  -Dsonar.login=cc34a066733b841dbaa231da2de2f6081dc8b356
                }
            }
        }
        stage('Push to Artifactory') {
            steps {
                echo 'Publishing to artifactory going on...'
		nexusArtifactUploader credentialsId: 'NexusCreds', groupId: 'com.example', nexusUrl: "${NEXUS_URL}", nexusVersion: 'nexus3', protocol: 'http', repository: 'test-war-practice', version: '1.1.0'    
                echo 'Uploaded'
            }
        }
        stage('Deploy to tomcat') {
            steps {
                echo 'Deploying going on...'
                deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: "${TOMCAT_URL}")], contextPath: "${CONTEXT_PATH}", onFailure: false, war: "${WAR_FILE_PATH}"
                echo 'Uploaded'
            }
        }
    }
}

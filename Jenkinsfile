pipeline {
    agent any

    triggers {
        githubPush()
    }
    environment {
        GITHUB_REPO_URL = 'https://github.com/srinivasmalladi9/mindcircuit15d.git'
        GIT_HUB_BRANCH = 'main'
        CONTEXT_PATH = 'app'
        WAR_FILE_PATH = '**/mindcircuitbatch15d-1.0.0.war'
        TOMCAT_URL = ''

    }

    stages {
        stage ('Clean WorkSpace') {
            steps {
                echo 'cleaning workspace....'
                cleanWs()
                echo 'cleaning cleanup completed'
            }  
        } 
        stage ('Continuous Integrtaion') {
            steps {
                echo 'Continuous Integration going on ....'
                git branch: "${GIT_HUB_BRANCH}", url: "${GITHUB_REPO_URL}"
                echo 'Continuous Integration completed'
            }
        }
        stage ('Continuous Build') {
            steps {
                echo ('Continuous Build going on...')
                sh 'mvn clean install'
                echo ('Continuous Build Completed')
            }
        }
        stage ('Deploy To Tomcat') {
            steps {
                echo ('Deployment going on...')
                deploy adapters: [tomcat9(credentialsId: 'Tomcatcreds', path: '', url:"${TOMCAT_URL}")], contextPath: "${CONTEXT_PATH}", onFailure: false, war: "${WAR_FILE_PATH}"
                echo ('Deployment Completed')
            }
        }
    }
}

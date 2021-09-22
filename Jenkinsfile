pipeline {
    agent { label 'GOL'}
    environment {
        CI_ENV = 'DEV'
    }
    triggers {
        cron('H * * * *')
        pollSCM('* * * * *')
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to build' )
    stages {
        stage('scm') {
            steps {

                git branch: 'master', url: 'https://github.com/Niranjjan7/game-of-life.git'

                echo env.CI_ENV
                echo env.DUMMY
            }
        }
        stage('build') {
            steps {
                echo env.GIT_URL
                timeout(time:10, unit: 'MINUTES') {
                    sh "mvn ${params.GOAL}"
                }
                stash includes: '**/gameoflife.war', name: 'golwar'
            }
        }
        stage('devserver'){
            agent { label 'UBUNTU,'}
            steps {
                unstash name: 'golwar'
    post {
        success {
            archive '**/gameoflife.war'
            junit '**/TEST-*.xml'
        }
        mail subject: 'BUILD Completed Successfully '+env.BUILD_ID, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'
        }
        failure {
            mail subject: 'BUILD Failed '+env.BUILD_ID+'URL is '+env.BUILD_URL, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'
        }
        always {
            echo "Finished"
        }
        changed {
            echo "Changed"
        }
        unstable {
            mail subject: 'BUILD Unstable '+env.BUILD_ID+'URL is '+env.BUILD_URL, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'
    }
}
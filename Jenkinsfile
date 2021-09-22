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
                sh 'mvn package'
            }
        }
        
    post {
        success {
            archive '**/gameoflife.war'
            junit '**/TEST-*.xml'
        }
        
    }
}
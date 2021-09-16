node('GOL') {
    stage('scm') {
        git 'https://github.com/Niranjjan7/game-of-life.git'
    }
    stage('build') {
        sh 'mvn clean package'
    }
    stage('postbuild') {
        junit '**/TEST-*.xml'
        archive '**/*.war'
    }
}
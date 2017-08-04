pipeline {
    agent { docker 'maven:3.3.3' }
    stages {
        stage('build') {
            steps {
                sh '''
                    pwd
                    ls -la
                    cd dt-ejb
                    pwd
                    ls -la
                    '''
                sh 'mvn clean verify'
            }
        }
    }
}

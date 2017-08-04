pipeline {
    agent { docker 'maven:3.3.3' }
    stages {
        stage('build') {
            steps {
                sh '''
                   cd dt-ejb
                   mvn clean verify
                   '''
                sh '''
                   cd Rest
                   mvn clean verify
                   '''
                sh '''
                   cd web
                   mvn clean verify
                   '''
                sh '''
                   cd daytrader-eeb
                   mvn clean verify
                   '''
             }
        }
    }
}

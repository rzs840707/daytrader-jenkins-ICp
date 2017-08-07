pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh '''
                   cd dt-ejb
                   mvn clean install
                   cd ../Rest
                   mvn clean install
                   cd ../web
                   mvn clean install
                   cd ../daytrader-ee6
                   mvn clean verify
                   '''
             }
        }
        stage('deploy') {
            steps {
                sh '''
                    docker login -u admin -p admin master.cfc:8500
                '''
            }
        }
    }
}

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
        stage('tag-and-push') {
            steps {
                sh '''
                    docker login -u admin -p admin master.cfc:8500
                    docker tag dhvines/daytrader-ee6:0.0.1-SNAPSHOT master.cfc:8500/default/daytrader-ee6
                    docker push master.cfc:8500/default/daytrader-ee6
                '''
            }
        }
    }
}

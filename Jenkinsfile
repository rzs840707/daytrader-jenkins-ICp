pipeline {
    agent { docker 'maven:3.3.3' }
    stages {
        stage('build') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                sh 'cd dt-ejb'
                sh 'mvn clean verify'
            }
        }
    }
}

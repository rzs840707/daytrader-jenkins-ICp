pipeline {
    agent any
    stages {
        stage('Continuous Integration') {
            steps {
                sh '''
                   #cd dt-ejb
                   #mvn clean install
                   #cd ../Rest
                   #mvn clean install
                   #cd ../web
                   #mvn clean install
                   #cd ../daytrader-ee6
                   #mvn clean verify
                   #cd ..
                   '''
             }
        }
        stage('Continuous Delivery') {
            steps {
                sh '''
                    docker login -u admin -p admin master.cfc:8500
                    docker tag daytrader-ee6:0.0.1-SNAPSHOT master.cfc:8500/default/daytrader-ee6
                    docker push master.cfc:8500/default/daytrader-ee6
                    kubectl config set-cluster cfc --server=https://172.21.20.136:8001 --insecure-skip-tls-verify=true
                    kubectl config set-context cfc --cluster=cfc
                    kubectl config set-credentials user --username admin --password admin
                    kubectl config set-context cfc --user=user --namespace=default
                    kubectl config use-context cfc
                    ls -la
                    kubectl delete deployment wlp-daytrader-jenkins
                    kubectl create -f app.json
                    kubectl create -f service.json
                '''
            }
        }
    }
}

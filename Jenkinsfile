pipeline {
    agent any
    stages {
        stage('Continuous Integration') {
            steps {
                sh '''
                   cd dt-ejb
                   mvn clean install
                   cd ../Rest
                   mvn clean install
                   cd ../web
                   mvn clean install
                   cd ../daytrader-ee6
                   mvn clean verify -Pdocker
                   cd ..
                   '''
             }
        }
        stage('Continuous Delivery') {
            steps {
                sh '''
                    echo "docker login to master.cfc"
                    docker login -u admin -p admin master.cfc:8500
                    echo "docker tag"
                    docker tag daytrader-ee6:0.0.1-SNAPSHOT master.cfc:8500/default/daytrader-ee6
                    echo "docker push"
                    docker push master.cfc:8500/default/daytrader-ee6
                    echo "kubectl login"
                    kubectl config set-cluster cfc --server=https://172.21.20.136:8001 --insecure-skip-tls-verify=true
                    kubectl config set-context cfc --cluster=cfc
                    kubectl config set-credentials user --token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImRlZmF1bHQtdG9rZW4tbWNmZm0iLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGVmYXVsdCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjBjNGEzOWY2LTc2YzEtMTFlNy1hODhjLTAwNTA1NjlhNGJhMiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmRlZmF1bHQifQ.Jknkvowh-UF-4mgmi0J5BsrSvpzX8yazAceONtQZ4pb0ZGsOOTcQuClc4KQeszAbBqjOLbj0f9FchphAWfhRU02hLaNqsPRIgBL4HhnJNVUrt5Yox5N2z5EWoDLtVLGP6-Ulkp_7L-wEva2uNd4GI7utbr8wNytO6Fkcym0szmGkKJXJjEuIzH6x4P5dI61uPgbyy3XSEpvgkl1sNGK9LqpFA-GGFg6Lpa6Jt09MDV6XU3eY7ELYx7YzI1QYaNH9GkiinimoU8m9QRuw_NFsQLRGM6w2JIwVRf1Ne1R_7QK6fVIQAw8_k9tNwrSgYCYQEpftH4UebmNx0gmsiu-40A
                    kubectl config set-context cfc --user=user --namespace=default
                    kubectl config use-context cfc
                    #!/bin/bash
                    echo "checking if wlp-daytrader-jenkins already exists"
                    if kubectl describe deployment wlp-daytrader-jenkins; then
                        echo "Application already exists, delete first"
                        kubectl delete service wlp-daytrader-jenkins
                        kubectl delete deployment wlp-daytrader-jenkins
                    fi
                    echo "Create application"
                    kubectl create -f app.json
                    echo "Create service"
                    set +e
                    kubectl create -f service.json                                      
                    echo "finished"
                '''
            }
        }
    }
}

podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
        containers:
        - name: centos
          image: centos
          command:
          - sleep
          args:
          - 99d
          restartPolicy: Never
    '''){
        node(POD_LABEL){
            stage('k8'){
                git 'https://github.com/austineisele/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git' 
                container('centos'){
                    stage('start calculator'){
                        sh '''
                        cd Chapter08/sample1
                        curl -ik -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/api/v1/namespaces/devops-tools/deployments -XPOST -H "Content-type: application/yaml" --data-binary @hazelcast.yaml
                        curl -k -H "Authorization: Bearer $(cat /var/run/secretes/kuberntes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/apis/apps/v1/namespaces/devops-tools/deployments -XPOST -H "Content-type: application/yaml" --data-binary @calculator.yaml
                            '''
                    }
                } 
            }
      }
}

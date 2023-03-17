podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
        containers:
        - name: gradle 
          image: 
          command:
          - sleep
          args:
          - 99d
          restartPolicy: Never
    '''){
        node(POD_LABEL){
            stage('gradle'){
                git 'https://github.com/austineisele/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git' 
                container('gradle'){
                    stage('test calculator'){
                        sh '''
                        cd Chapter09/sample3
                        chmod +x gradlew
                        ./gradlew acceptanceTest -Dcalculator.url=https://calculator-service:8080
                            '''
                    }
                } 
            }
      }
}

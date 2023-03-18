podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
        containers:
        - name: gradle 
          image: gradle:jdk8 
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
                        ./gradlew acceptanceTest -Dcalculator.url=http://calculator-service:8080
                            '''

                            publishHTML(target: [
                              reportDir: 'Chapter09/sample3/build/reports/tests/acceptanceTest',
                              reportFiles: 'index.html',
                              reportName: "Cucumber Acceptance Test Report"
                            ])
                    }
                } 
            }
      }
}

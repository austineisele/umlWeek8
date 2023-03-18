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
            stage('Build Services and Test'){ 
              git 'https://github.com/austineisele/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git' 
                container('gradle'){
                    stage('start calculator'){
                        sh '''
                        pwd
                        cd Chapter08/sample1
                        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                        chmod +x ./kubectl
                        ./kubectl apply -f calculator.yaml
                        ./kubectl apply -f hazelcast.yaml
                            '''
                    }
                    stage('test calculator'){
                        sh '''
                          pwd
                          cd ../../Chapter09/sample3
                          chmod +x gradlew
                          ./gradlew acceptanceTest -Dcalculator.url=http://calculator-service:8080
                          '''
                          publishHTML(target: [
                            reportDir: 'Chapter09/sample3/reports/tests/acceptanceTest',
                            reportFiles: 'index.html',
                            reportName: "Cucumber Acceptance Test Report"
                          ])
                    } 
            }
      }
    }
}

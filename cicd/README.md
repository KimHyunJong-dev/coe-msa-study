# CI/CD (Continuous Integration and Continuous Delivery)
jenkins를 활용한 CICD구성에 대하여 설명한다.

## 1. Jenkins Plugins
### Build monitor
빌드 상태를 모니터링 할 수 있는 뷰를 제공한다.  
[Learn more](https://wiki.jenkins.io/display/JENKINS/Build+Monitor+Plugin)

<img src="./images/buildMonitor.png" alt="drawing" style="width: 800px;"/>

### PMD
PMD 수행 결과 보고서 조회 및 트랜드 챠트를 조회 할 수 있다.  
[Learn more](https://wiki.jenkins.io/display/JENKINS/PMD+Plugin)

[PMD Trend]  
<img src="./images/pmdTrend.png" alt="drawing" style="width: 400px;"/>

[PMD Result]  
<img src="./images/pmdResult.png" alt="drawing" style="width: 800px;"/>

- 설정 방법  
Jenkiks > Job > configure > Post-build Actions에서 Publish PMD analysis results 항목에 결과 파일명 지정

### FindBugs
FindBugs 수행 결과 보고서 조회 및 트랜드 챠트를 조회 할 수 있다.  
[Learn more](https://wiki.jenkins.io/display/JENKINS/FindBugs+Plugin)
- 설정 방법  
Jenkiks > Job > configure > Post-build Actions에서 Publish FindBugs analysis results 항목에 결과 파일명 지정

### Slack
빌드 결과를 slack으로 전송할 수 있다.  
[Learn more](https://wiki.jenkins.io/display/JENKINS/Slack+Plugin)

### Blue Ocean
젠킨스의 개선된 UI를 사용가능하고, Pipeline을  단계별로 구성 할 수 있는 도구를 제공한다.   
[Learn more](https://jenkins.io/projects/blueocean/)

## 2. Git Webhook
Git 에서의 작업 조건을 트리거로 해서 빌드를 수행 한다.
- 설정 방법  
 [Github]  
 <img src="./images/webHookGit.png" alt="drawing" style="width: 600px;"/>

 [Jenkins]  
 <img src="./images/webHookJob1.png" alt="drawing" style="width: 600px;"/>

 <img src="./images/webHookJob2.png" alt="drawing" style="width: 600px;"/>

## 3. Pipeline
젠킨스를 이용한 빌드파이프라인 구성이 가능하다.  
각자의 프로젝트 상황에 맞는 단계로 파이프 라인을 구성 한다.

(아래는 5개의 단계로 구성된 파이프라인 script 예시를 사용한다.)
<img src="./images/pipeline.png" alt="drawing" style="width: 800px;"/>
- Declarative: Tool install  
빌드에 사용 될 Tool 지정

- Git checkout and Static analytics   
Source 체크아웃 및 정적 분석 실행

- Test and build  
Test와 build 실행

- Pact verify  
Contract Test 수행(Pact Broker사용)

- Dev deploy and create swagger docs  
artifact를 배포 하고 API 관련 문서를 생성

> Pipeline 스크립트는 아래 메뉴에서 참고하고 생성 가능  
Jenkins > job > pipeline syntext
```
    pipeline {
       tools{
           maven "M3"
       }
       agent any
       stages{
         stage('Git checkout and Static analytics') {
             steps{
                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'bfb6f0b7-256b-47a1-88bf-f33dd42eb0fb', url: 'https://github.com/SDSACT/microservice-coe-order-service.git']]])
                 checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'testmin0418', url: 'https://github.com/SDSACT/microservice-coe-order-service.git']]]

                 script {
                      if(params.pmdYn){
                          sh "mvn pmd:pmd"
                          pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: 'target/pmd.xml', unHealthy: ''
                      }
                 }
             }

         }

         stage('Test and build'){
             steps{
                 script{
                     if(params.unitTestYn){
                          sh "mvn -Dtest=**/unit/**/* -DfailIfNoTests=false test"  

                     }
                     if (params.functionalTestYn) {
                         sh "mvn -Dtest=**/functional/**/* -DfailIfNoTests=false test"
                     }
                     sh "mvn -Dtest=**/contract/**/* -DfailIfNoTests=false test"
                     sh "mvn pact:publish"
                      junit '**/target/surefire-reports/TEST-*.xml'
                      sh "mvn clean install -DskipTests=true"
                 }
             }

         }
         stage('Pact Verify') {
             steps {
                 sh 'curl -o customer-service.jar  -u admin:admin123 -L "http://localhost:8081/nexus/service/local/artifact/maven/redirect?r=coe&g=org.coe.msa&a=customer-service&v=0.0.1&p=jar"'
                 sh 'nohup java -Dserver.port=10000 -jar customer-service.jar &'
                  waitUntil {
                     script {
                       def r = sh script: 'curl http://localhost:10000', returnStatus: true
                       return (r == 0);
                     }
                  }
                 sh 'mvn pact:verify'
             }
         }
         stage('Dev deploy and create swagger docs') {
             steps {
                 sh 'scp -i $KEY_PATH ./target/order-service-0.0.1-SNAPSHOT.jar /Users/santiago/runscript.sh  actmember@192.168.30.129:/Users/actmember/microservice-coe'
                 sh 'nohup ssh -i /Users/santiago/.ssh/oslo.key actmember@192.168.30.129 "cd /Users/actmember/microservice-coe/ && ./runscript.sh restart order-service-0.0.1-SNAPSHOT.jar 9997" &'
                 waitUntil {
                     script {
                       def r = sh script: 'curl http://192.168.30.129:9997', returnStatus: true
                       return (r == 0);
                     }
                  }
                  sh 'curl -sb -H "Accept: application/json" "http://192.168.30.129:9997/v2/api-docs" > /Users/santiago/microservice-coe-ci/swagger-json/order-api-docs.json'
                  sh 'cd /Users/santiago/microservice-coe-ci/swaggregator && /Users/santiago/.nvm/versions/node/v6.14.1/bin/node swaggregator.js'
                  sh 'cp /Users/santiago/microservice-coe-ci/swaggregator/dist/aggregated.json /Users/santiago/microservice-coe-ci/swagger-ui/dist'
             }
         }
       }
     }
```

## 4. Pact(Contract Test) / Nexus / Pact Broker
Client Driven Test 로서 Client가 필요로 하는 API 항목에 대하여 Contract(Pact)를 생성하고, 이에 대한 Provider의 적합 여부를 Test 한다.

<img src="./images/consumerPact.png" alt="drawing" style="width: 800px;"/>

<img src="./images/providerPact.png" alt="drawing" style="width: 800px;"/>
## 5. Ansible

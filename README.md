# Cloud App. 설계    
1. Microservice Outer/Inner Architecture 수립
    1. Microservice 구성내용
    1. BFF
        1. 참조 : http://34.117.35.195/operation/architecture/architecture-one/
        2. API Gateway + BFF 패턴 
        - ![image](https://user-images.githubusercontent.com/66579939/126030374-fbd729bd-7f7a-40b5-ba01-883b304898e3.png)
        - API Gateway를 나누어서 클라이언트 앱당 하나씩 분할 : 단일 API Gateway가 어플리케이션의 모든 내부 마이크로서비스를 호출 하는 경우, API Gateway는 거대해지게 되고, 모놀리틱 아키텍처에서 발생했던 문제점이 또 다시 발생 가능성 존재
    1. CI/CD
    - CI(Continuous Integration) : 여러 개발자들의 코드를 계속해서 통합하는 것
    - CD(Continuous Delivery) : 개발자들이 코드를 작성하면 사용자들이 사용할 수 있도록 만드는 것
    - ==> 지속적으로 배포 가능한 상태 유지
    - 참조 : http://34.117.35.195/operation/deployment/deployment-one/
    1. 헥사고널 아키텍처
        - 참조 : https://engineering-skcc.github.io/microservice%20inner%20achitecture/inner-architecture-2/
        - Port and Adapters Architecture 라고도 한다.
        - 내부영역 : 비즈니스 로직 표현, 기술 독립적인 영역이며 외부영역과 연계되는 포트를 가짐
        - 외부영역 : 인터페이스 처리 담당
        - Inbound Adapter : 외부 요청 처리 (REST API 발행하는 컨트롤러, Spring MVC 컨트롤러, 이벤트 메시지 핸들러)
        - Outbound Adapter : 비즈니스 로직에 의해 호출되어 외부와 연계 (DAO, 이벤트 메시지 발행 클래스)
        * 헥사고널 아키텍처 그림
     ![image](https://user-images.githubusercontent.com/66579939/126024705-9a1d3025-937a-4809-b706-4a1b3bfb8d71.png)
    1. 레이어드 아키텍처
        - Spring 에서 일반적으로 사용되는 아키텍처
        - 도메인 Logic 에 집중
        ```
        [Presentaion Layer] 컨트롤러
        [Business Logic Layer] 서비스 - 도메인
        [Data Access Layer] DAO
        ```
    1. Restful API 설계 원칙
        1. REST : Representational State Transfe라는 용어의 약자이다. 자원을 URI로 표시하고 해당 자원의 상태를 주고 받는 것을 의미한다.
        2. REST API 설계 규칙
            - 참조 : https://velog.io/@stampid/REST-API%EC%99%80-RESTful-API
            - URI 는 정보의 자원을 표현한다. (자원의 이름은 명사 사용, 행위 표현(동사) X)
            - 자원에 대한 행위는 HTTP METHOD 로 표현
            - 슬래시 (/) 는 계층 관계를 나타내는데 사용 
           
        ```
        CRUD	    HTTP METHOD	URI
        user들을 표시	GET	/users
        user 하나만 표시	GET	/users/:id
        user를 생성	POST	/users
        user를 수정	PUT	/users:id
        user를 삭제	DELETE	/users:id
        ```
 1. Microservice 기반 (Base, Backing) 서비스 활용
    1. 기반 및 Backing 서비스 : Spring Cloud 활용
        # Spring Cloud Config : 어플리케이션 동작 시 개발, 운영 환경 등 설정 파일을 선택할 수 있도록 처리
        - Spring Cloud Zookeeper(Apache Zookeeper)
        - 서킷 브레이커(Circuit Breaker)
            장애 발생한 인스턴스로 가는 요청을 '중단' 시킴으로써 장애 발생 회피
        - Resilience4J
    3. Polyglot 관점에서 Spring Cloud 사용 방법
 1. BIZ Microservice 식별
    1. Bounded Context/Context Map
    2. 도메인 주도 설계 (DDD)
        - 참조 : https://huisam.tistory.com/entry/DDD
        - 도메인 : 실제 사건이 발생하는 집합 (Reservation Domain, Payment Domain, Playground Domain...)
        - 여러 도메인들이 서로 상호작용하며 설계하는 것이 도메인 주도 설계이다.
        - 특징은 같은 객체 (Object or Class) 여러 개가 존재할 수 있다는 것.
        - DDD를 서비스에 적용 시킨 것이 Microservice 이다.
        - 각각의 도메인은 분리되고, 서로 API 호출 (높은 응집력, 낮은 결합도) => 변경과 확장에 용이하다.
        - Aggregate : 도메인 영역을 대표하는 객체
    4. 도메인 모델 패턴 / 데이터 모델 패턴의 차이점
 1. BiZ Microservice 상세 설계
    1. 레이어 역할에 맞는 비즈 설계 : API 설계, 데이터 모델링, 객체 모델링
 1. 서비스 API 설계
    1. Restful API
    2. 마이크로 서비스가 적합한 환경의 특징
        - Container 기반 Cloud 환경
        - Domain Service 중심으로 Container Scale In/Out 
        - 서비스를 분리하여 시장의 빠른 변화에 민첩하게 대응 가능함
        - DevOps 환경
 1. Data Considency (데이터 일관성) 전략 수립
    1. DB 분할 방법 : 비동기 이벤트 기반 SAGA 패턴
    - SAGA 패턴 : 마이크로서비스의 독립적인 분산 트랜잭션 처리를 지원하는 패턴
    - 각 서비스의 로컬 트랜잭션을 순차적으로 처리하는 패턴
    - 여러 개의 서비스를 1개의 트랜잭션으로 묶지 않고 각 로컬 트랜잭션과 보상 트랜잭션을 이용해 비즈니스/데이터의 정합성을 맞춘다.
    - 결과적 일관성(eventual consistency) : 데이터 일관성이 실시간을 아니더라도 일정 시점이 됐을 때 일관성 만족
    - SAGA 패턴 + 이벤트 메시지 기반 비동기 통신 적용
        - 마이크로서비스 트랜잭션은 독립적
        - 각 트랜잭션 성공 시 상태 변경 이벤트 발행해 이 이벤트를 구독하는 다른 서비스의 로컬 
    3. CQRS 패턴 (Command and Query Responsibility Segregation)
        - 참조 : http://34.117.35.195/operation/integration/integration-six/
        - 명령(Command, DB업데이트) 및 쿼리(Query, DB Read) 의 책임을 분리하는 패턴
            1. 장점 : 성능, 확장성 보안을 극대화
            2. CQRS 구현 (이벤트 소싱)
                - 이벤트 소싱은 이벤트 자체를 DB 처럼 사용하는 방식입니다. CUD 명령은 기존의 DB 에 저장을 한 후, 이벤트를 발생시켜서 쿼리용 데이터를 따로 만드는 방식입니다. CQRS 의 가장 큰 시너지를 낼수 있는 방식이라 최근에는 필수적인 설계 방식이 되었습니다.
            ```
            // 개인 회원 신규/변경시 개인회원 Entity를 저장한다    
            } else if( PersonalMemberChanged.getEventType().equals(PersonalMemberChanged.class.getSimpleName())){
                PersonalMemberChanged personalMemberChanged = objectMapper.readValue(message, PersonalMemberChanged.class);

                PersonalMember personalMember = new PersonalMember();
                personalMember.setPersonalId(personalMemberChanged.getId());
                personalMember.setEmail(personalMemberChanged.getEmail());
                personalMember.setMemberFavoriteType(personalMemberChanged.getFavoriteType());
                personalMember.setMemberLevelType(personalMemberChanged.getLevelType());
                personalMember.setMemberStatusType(personalMemberChanged.getStatusType());
                personalMember.setMileage(personalMemberChanged.getMileage());
                personalMember.setMobile(personalMemberChanged.getMobile());
                personalMember.setName(personalMemberChanged.getName());
                personalMember.setPoint(personalMemberChanged.getPoint());

                personalMemberRepository.save(personalMember);
            }
            ```

# Cloud App. 구현    
1. 환경 설정
    1. Maven 개발 환경 설정
        - 참조1 : https://m.blog.naver.com/dktmrorl/222131777444
        - 참조2 : https://goddaehee.tistory.com/199
    ```
    Maven 프로젝트 구조
    - pom.xml
    - src
        - main
            - java
                - com.ss501.service
            - resources
        - test
            - java
                - com.ss501.service
            - resources
    ```
1. Container
    1. Docker, Kubenetes, Jenkins 를 이용한 단계별 배포 과정 설명
    2. Kubernetes Ingress (Load Balancer)
        * Ingress 
        ![image](https://user-images.githubusercontent.com/66579939/126025444-73f1119a-b7e0-4133-a929-6ad740fd6e62.png)
        - 참조 : https://kubernetes.io/ko/docs/concepts/services-networking/ingress/
        - 서비스가 아닌 컨트롤러(ingress-controller)가 직접 외부 통신을 수행
        - 같은 ip 주소를 외부에 노출
        - 인그레스 : 클러스터 외부 => 내부로 접근하는 요청을 어떻게 처리할 지 정의한 규칙의 모임
        - Ingress는 외부로부터 들어오는 요청에 대한 로드밸런싱, TLS/SSL 인증서 처리, 도메인 기반 가상 호스팅 제공, 특정 HTTP 경로의 라우팅 등의 규칙들을 정의해 둔 자원이며, 이런 규칙들을 실제로 동작하게 해주는건 Ingress-Controller다. 
        * ingress object (myplay-configuration/configuration/myplay-ingress.yaml)
        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          annotations:
            kubernetes.io/ingress.class: public-nginx
          name: myplay-ingress
          namespace: myplay
        spec:
          rules:
          - host: myplay.factory-dev.cloudzcp.com
            http:
              paths:
              - backend:
                  serviceName: myplay-bff-app-dev
                  servicePort: 8080
                path: /
        ```
1. 개발 패턴 (MVC, MVVM 의 차이점)
    1. 참조1 : https://beomy.tistory.com/43
    2. 참조2 : https://velog.io/@addiescode/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-MVC-MVVM
    3. MVC 패턴 : Model + View + Controller
        - Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
        - View : 사용자에게 보여지는 UI 부분
        - Controller : 사용자의 입력(Action)을 받고 처리하는 부분
        - MVC 모델 단점 : View 와 Model 의존성 분리 X
    4. MVVM 패턴 : Model + View + View Model
        - View Model : View 를 표현하기 위해 만든 View 를 위한 Model이다. View를 나타내기 위한 데이터 처리.
        - MVVM 패턴은 Command 패턴과 Data Binding 두가지 패턴을 사용함
        - View 와 Model 사이의 의존성을 없앰
3. 데이터 핸들링
    1. 데이터 핸들링 시 사용하는 ORM 내용과 장단점
        - 참조 : https://geonlee.tistory.com/207
        - ORM은 Object Relational Mapping 즉, 객체-관계 매핑의 줄임말이다.
        - 객체 클래스와 데이터 테이블을 자동으로 매핑하는 것을 의미한다. (SQL문 자동 생성)
        - SQL 매핑하는 것이 아닌, DB 테이블 매핑.
    2. ORM 장점
        - SQL문 X, 클래스 메서드로 DB 조작 가능 => 객체지향적인 코드
        - 재사용, 유지보수, 리팩토링 용이
        - DBMS 종속성 하락 (Object 와 RDB 간의 패러다임 불일치 해결)
    3. ORM 단점
        - 프로젝트의 복잡도가 커지면 ORM 구현 난이도가 올라감
        - 속도 저하 및 일관성을 무너뜨리는 문제점 발생 가능
        - 대형 SQL문의 경우 속도를 위한 튜닝이 필요하여 결국 SQL을 이용해야 함.
    4. JPA (Java Persistence API)
        - 참조 : https://velog.io/@adam2/JPA%EB%8A%94-%EB%8F%84%EB%8D%B0%EC%B2%B4-%EB%AD%98%EA%B9%8C-orm-%EC%98%81%EC%86%8D%EC%84%B1-hibernate-spring-data-jpa
        - 자바 ORM 기술 표준 인터페이스 (JPA 표준 명세 구현 : Hibernate)
        ![image](https://user-images.githubusercontent.com/66579939/126027734-6c0be30b-326e-44ce-9eeb-437c6e28252f.png)
        - JPA 는 애플리케이션과 JDBC 사이에서 동작한다.
        - JPA 사용하면, JPA 내부에서 JDBC API 사용하여 SQL 호출, DB 통신
4. 환경 이해 (Dev, Staging, Production)
    1. 환경에 따른 설정 정보
        - 참조 : https://multifrontgarden.tistory.com/277
        - spring.config.active.on-profile 사용 (Spring Cloud)
    1. Myplayground 환경
        - 프로젝트 Cloud 환경에서는 dev profile 을 사용한다.
        - Local 환경에서는 profile 별도 명시하지 않았으므로 default profile 로 
        - configuration application.yaml 설정
        ```
        server:
          port: 8080
        spring:
          profiles:
            include: common,custom,test,dev
        ```
        - 서비스별 application.yaml profile 설정
        ```
        spring:
          config:
            activate:
              on-profile: dev
        ```
5. Cloud App Back-End 구현
    1. Spring Boot 비즈 로직 개발
    2. 기반 서비스 연계 (Spring Cloud Connector)
    3. 저장소 처리 구현
    
# Cloud App. 운영
1. Auto-Scaler Policy
    1. 오토스케일링 정책과 임계치 내용
    ```
    hpa
    ```
1. CI/CD 적용
    1. Harbor (Image Registry)
        - 오픈소스 컨테이너 이미지 레지스트리
        - 역할 기반 접근 제어, 이미지 취약점 스캐닝, 이미지 서명 등의 기능
    2. Jenkins 설정
        - 빌드/배포 실행에 필요한 권한 설정, Kubernetes를 위한 Pipeline 작성
        - ZCP 사용자그룹(namespace), 권한이 적용됨
    
1. Jenkins : Java Runtime 위에서 동작하는 자동화 서버
    - 다양한 플러그인을 종합하여 CI/CD Pipeline 을 만들어서 자동화 작업을 가능하게 함
    1. Jenkins 파이프라인
        - Git Checkout, Source Build, Docker Image Build, Deploy
    ![cicd](https://user-images.githubusercontent.com/66579939/126031109-f6ced7db-068b-4bbb-95c8-1aa4a319ef7d.png)
    ```
        timestamps {
        podTemplate(label:label,
            serviceAccount: "zcp-system-sa-${ZCP_USERID}",
            containers: [
                containerTemplate(name: 'maven', image: 'maven:3.6.3-jdk-11-slim', ttyEnabled: true, command: 'cat'),
                containerTemplate(name: 'docker', image: 'docker:17-dind', ttyEnabled: true, command: 'dockerd-entrypoint.sh', privileged: true),
                containerTemplate(name: 'tools', image: 'argoproj/argo-cd-ci-builder:v1.0.0', command: 'cat', ttyEnabled: true),
                containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.18.2', ttyEnabled: true, command: 'cat'),
                containerTemplate(name: 'kustomize', image: 'gauravgaglani/k8s-kustomize:1.1.0', ttyEnabled: true, command: 'cat')
            ],
            volumes: [
                persistentVolumeClaim(mountPath: '/root/.m2', claimName: 'zcp-jenkins-mvn-repo')
            ]) {

            node(label) {
                stage('SOURCE CHECKOUT') {
                    dir('APP_SRC_WORKSPACE') {
                        checkout scm: [ \
                            $class : 'GitSCM', branches: [[name: '*/master']], \
                            submoduleCfg: [], \
                            userRemoteConfigs: [[url: "https://${REPO_URL}", credentialsId: 'GIT_CREDENTIALS',]] \
                        ]
                    }
                }

                stage('BUILD') {
                    dir('APP_SRC_WORKSPACE') {
                        container('maven') {
                            mavenBuild goal: 'clean package -DskipTests=true -f pom.xml', systemProperties:['maven.repo.local':"/root/.m2/${JOB_NAME}"]
                        }
                    }
                }

                stage('BUILD DOCKER IMAGE') {
                    dir('APP_SRC_WORKSPACE') {
                        container('docker') {
                            dockerCmd.withDockerRegistry([credentialsId:'HARBOR_CREDENTIALS', url:"https://${HARBOR_REGISTRY}"]) {
                                def app = docker.build("${HARBOR_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_VERSION}")
                                app.push()
                                app.push("latest")
                            }
                        }
                    }
                }

                stage('CONFIG CHECKOUT') {
                    dir('CONFIG_SRC_WORKSPACE') {
                        checkout scm
                    }
                }
                // kustomize 기반 kubenetes 배포 리소스 (deployment, service, configmap) 자동화하여 
                stage('BUILD K8S YAML') {
                    dir('CONFIG_SRC_WORKSPACE') {
                        container('kustomize') {
                            sh "kustomize build --load_restrictor none --enable_kyaml=false ${DEPLOY_PATH} > deploy.yaml"
                            echo 'Kubernetes 배포 yaml 생성 내역'
                            sh 'cat deploy.yaml'
                        }
                    }
                }

                stage('DEPLOY') {
                    dir('CONFIG_SRC_WORKSPACE') {
                        container('kubectl') {
                            kubeCmd.apply file: 'deploy.yaml', wait: 300, recoverOnFail: false, namespace: K8S_NAMESPACE
                            sh "kubectl rollout restart deployment ${DEPLOYMENT} -n ${K8S_NAMESPACE} "
                        }
                    }
                }
            }
        }
    }
    ```  
    1. Kustomize (참고 : https://www.jacobbaek.com/1172)
        - Kubernetes resource 설정을 customizing 할 수 있는 도구  
        - kubectl 1.14 부터 포함되어 사용이 가능함
        - kustomization.yaml 파일을 사용하여 불러올 yaml 지정, 기존 Application 배포에 사용하는 yaml 파일 그대로 사용할 수 있게 해준다.
        - stage (production, staging, dev) 기반 데이터를 별도 관리 가능
        - 기본구조
        ```
        -base // 기본 yaml file 들이 저장되는 경로
            -deployment.yaml
            -kustomization.yaml
            -service.yaml
        -overlays // stage 별로 나누거나 공용으로 사용되는 base 에 특정 value 추가 시 사용
            -production
                -deployment.yaml
                -kustomization.yaml
            -staging
                -config.env
                -deployment.yaml
                -kustomization.yaml
        ```
    1. Secret, ConfigMap
    - 참조 : https://kubernetes.io/ko/docs/tasks/manage-kubernetes-objects/kustomization/
    - ConfigMap : kustomization.yaml 에서 ConfigmapGenerator 사용
        ```
        apiVersion: kustomize.config.k8s.io/v1beta1
        kind: Kustomization
        resources:
          - ../../../../base/deployment
          - ../resource/role.yaml # SA for dev environment
          - ../resource/sa.yaml
          - ../resource/secret.yaml
          - ../resource/configmap.yaml
        commonLabels:
          cluster-tier: dev
        configMapGenerator:
          - name: app-config
            behavior: merge
            files:
              - ../resource/application-common.yaml
        patches:
          - path: ../resource/backend-cred-patch.yaml
            target:
              group: apps
              version: v1
              name: app
          - path: ../resource/configmap-reader-patch.yaml
            target:
              group: apps
              version: v1
              name: app
        ```
    - application-common.yaml (maria db 주소를 configmap 으로 관리)
    ![image](https://user-images.githubusercontent.com/66579939/126035960-1e9194ba-2c2b-47d8-b807-c7a31629c33c.png)
    - backend-cred-patch.yaml 에서 configmap 참조 (configmap 위치는 kustomization.yaml 에 추가)
    ![image](https://user-images.githubusercontent.com/66579939/126035987-051c4912-cced-4f8e-88a9-d7e2301bcbbc.png)
    - /resources/configMap.yaml
    ![image](https://user-images.githubusercontent.com/66579939/126035997-654f8d6f-215d-47ad-a6b4-710ffc73cf3f.png)

    1. 블루그린 배포
    1. Canary 배포 패턴
1. 모니터링
    1. Grafana
    - 데이터 소스로부터 차트, 그래프, 알람 등을 웹 환경에서 제공해주는 interactive visualization web application
    - Grafana는 오픈소스 시각화 도구로 Prometheus를 지원하고 있으며, 이외에도 Graphite, InfluxDB, OpenTSDB, Elasticsearch, CloudWatch 등과 같은 도구와 연동할 수 있다.
    - Grafana를 사용하여 대시보드를 구성하기 위해서는 실제 매트릭을 수집하고 있는 데이터베이스가 필요하다.(Elasticsearch 혹은 Prometheus같은 DB)
    - Microservice Dashboard 를 통해 CPU, Memory, Network 사용량 등 확인 가능
    1. Prometheus (참조 : https://medium.com/finda-tech/prometheus%EB%9E%80-cf52c9a8785f)
    - 오픈소스 시스템 모니터링 및 경고 툴킷 (시각화 도구로 Grafana 사용)
1. 로깅 (참조 : https://velog.io/@hanblueblue/Elastic-Search-1)
    1. ElasticSearch
    - 루씬(Apache Lucene) 기반의 Full Text로 검색이 가능한 오픈소스 분석엔진. 주로 REST API를 이용해 처리한다. 대량의 데이터를 신속하게 (거의 실시간으로) 저장, 검색, 분석 할 수 있다.
    - 모든 데이터를 색인하여 저장하고 검색, 집계 등을 수행
    2. Logstash
    - 플러그인을 이용해 데이터 집계와 보관, 서버 데이터 처리를 담당한다. 
    - 파이프라인으로 데이터를 수집해 필터를 통해 변환 후 Elastic Search로 전송한다.
    3. Kibana (참조 : https://esbook.kimjmin.net/01-overview/1.1-elastic-stack/1.1.3-kibana)
    - ES와 연동되어 데이터를 시각화해주는 도구
        - Discover : ElasticSearch에 색인된 소스 데이터들의 검색을 위한 메뉴
        - Visualize : aggregation 집계 기능을 통해 조회된 데이터의 통계를 차트로 표현
        - Dashboard : Visualize 메뉴에서 만들어진 시각화 도구를 조합해서 대시보드 화면을 만든다.

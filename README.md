## Cloud App. 설계    
1. Microservice Outer/Inner Architecture 수립
    1. Microservice 구성내용
    2. BFF
        1. 참조 : http://34.117.35.195/operation/architecture/architecture-one/
        2. API Gateway + BFF 패턴
        - BFF
        ![image](https://user-images.githubusercontent.com/66579939/126030374-fbd729bd-7f7a-40b5-ba01-883b304898e3.png)
        - API Gateway를 나누어서 클라이언트 앱당 하나씩 분할 : 단일 API Gateway가 어플리케이션의 모든 내부 마이크로서비스를 호출 하는 경우, API Gateway는 거대해지게 되고, 모놀리틱 아키텍처에서 발생했던 문제점이 또 다시 발생 가능성 존재
    4. CI/CD
    5. 헥사고널 아키텍처
        - 참조 : https://engineering-skcc.github.io/microservice%20inner%20achitecture/inner-architecture-2/
        - Port and Adapters Architecture 라고도 한다.
        - 내부영역 : 비즈니스 로직 표현, 기술 독립적인 영역이며 외부영역과 연계되는 포트를 가짐
        - 외부영역 : 인터페이스 처리 담당
        - Inbound Adapter : 외부 요청 처리 (REST API 발행하는 컨트롤러, Spring MVC 컨트롤러, 이벤트 메시지 핸들러)
        - Outbound Adapter : 비즈니스 로직에 의해 호출되어 외부와 연계 (DAO, 이벤트 메시지 발행 클래스)
        * 헥사고널 아키텍처 그림
     ![image](https://user-images.githubusercontent.com/66579939/126024705-9a1d3025-937a-4809-b706-4a1b3bfb8d71.png)
    6. Restful API 설계 원칙
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
    1. 기반 및 Backing 서비스 : Spring Cloud 활용 (Resilience4J) 
    2. Polyglot 관점에서 Spring Cloud 사용 방법
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
    2. CQRS 패턴 (Command and Query Responsibility Segregation)
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

## Cloud App. 구현    
1. 환경 설정
    1. Maven 개발 환경 설정
    ```
    Maven 라이브러리, 빌드 등
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
        - spring.config.active.on-profile 사용
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
    
## Cloud App. 운영
1. Auto-Scaler Policy
    1. 오토스케일링 정책과 임계치 내용
    ```
    hpa
    ```
1. 블루-그린 배포, CI/CD
    1. Jenkins 파이프라인 내용
    ```
    kubectl describe pod [pod 명]   
    ```  
    1. 블루그린 배포
    2. Canary 배포 패턴
1. 모니터링
    1. Prometheus OSS 활용?
    2. 인스턴스 리소스 상태 확인
1. 로깅
    1. Kibana 로그 분석

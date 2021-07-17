## Cloud App. 설계    
1. Microservice Outer/Inner Architecture 수립
    1.Microservice 구성내용
    2.BFF
    3.CI/CD
    4.내부구조 설명 : 헥사고널 아키텍처
    5.내부구조 설명 : Restful API 설계 원칙
    ```
    server
    ```
 1. Microservice 기반 (Base, Backing) 서비스 활용
    1. 기반 및 Backing 서비스 : Spring Cloud 활용 (Resilience4J) 
    2. Polyglot 관점에서 Spring Cloud 사용 방법
 4. BIZ Microservice 식별
    1. Bounded Context/Context Map
    2. 도메인 주도 설계
    3. 도메인 모델 패턴 / 데이터 모델 패턴의 차이점
 6. BiZ Microservice 상세 설계
    1. 레이어 역할에 맞는 비즈 설계 : API 설계, 데이터 모델링, 객체 모델링
 8. 서비스 API 설계
    1. Restful API
    2. 마이크로 서비스가 적합한 환경의 특징
 10. Data Considency (데이터 일관성) 전략 수립
    1. DB 분할 방법 : 비동기 이벤트 기반 SAGA 패턴
    2. CQRS 패턴

## Cloud App. 구현    
1. 환경 설정
    1. Maven 개발 환경 설정
    ```
    Maven 라이브러리, 빌드 등
    ```
1. Container
    1. Docker, Kubenetes, Jenkins 를 이용한 단계별 배포 과정 설명
    2. Kubernetes Ingress (Load Balancer)
    ```
    Ingress
    ```
1. 개발 패턴 (MVC, MVVM 의 차이점)
1. 데이터 핸들링
    1. 데이터 핸들링 시 사용하는 ORM 내용과 장단점
1. 환경 이해 (Dev, Staging, Production)
    1. 환경에 따른 설정 정보
1. Cloud App Back-End 구현
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

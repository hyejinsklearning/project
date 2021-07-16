## Cloud App. 설계    
1. Microservice Outer/Inner Architecture 수립
    1. myplay-configuration/configuration/base/common/application.yaml - spring.profiles.include 에 dev 추가
    ```
    server
    ```
 1. Microservice 기반 (Base, Backing) 서비스 활용
 1. BIZ Microservice 식별
 1. BiZ Microservice 상세 설계
 1. 서비스 API 설계
 1. Data Considency (데이터 일관성) 전략 수립

## Cloud App. 구현    
1. 서비스별 application.yaml 서버 설정 시 아래처럼 profile 설정
    1. myplay-bff/src/main/resources/application.yml
    ```
    spring:
      config:
        activate:
          on-profile: dev
    ```
    1. (spring boot 2.4 버전 이상에서는 위 처럼 설정) 

## Cloud App. 운영
1. 서버에서 오류 발생 시 로그 확인 (kubectl 콘솔 명령어)
    1. kubectl get pod -n myplay
    ```
    NAME                                          READY   STATUS    RESTARTS   AGE
    myplay-bff-app-dev-778c7bf77d-6qbf2           1/1     Running   0          20m
    myplay-member-app-dev-6cd75dd7d9-vcrdr        1/1     Running   0          9h
    myplay-payment-app-dev-5f7dcb4685-gkxd8       1/1     Running   0          10d
    myplay-playground-app-dev-68b56d4dfc-5p8r7    1/1     Running   0          7h
    myplay-reservation-app-dev-857dcb7c5c-dlzs8   1/1     Running   0          7d1h
    myplay-review-app-dev-6f579796f8-6jt8j        1/1     Running   0          9h
    ```
    1. kubectl logs -f [위에서 조회한 pod 명]
    ```
    kubectl logs -f myplay-bff-app-dev-778c7bf77d-6qbf2
    ```

1. pod 가 안올라오는 경우 로그 확인
    ```
    kubectl describe pod [pod 명]   
    ```  

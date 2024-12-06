# 1장. 사용자 수에 따른 규모 확장성

## (1) 단일서버

![1.single_server.JPG](../images/1.single_server.JPG)

1. 사용자가 도메인 이름을 이용하여 웹사이트에 접속
2. 접속을 위해서 DNS에 질의하여 IP주소로 변환하는 과정 필요(DNS는 보통 외부시스템)
3. 반환된 IP주소(123.456.123.101)으로 HTTP요청 전달
4. 요청받은 웹 서버는 HTML페이지나 JSON형태의 응답 반환

## (2) 데이터베이스
![1.database.JPG](../images/1.database.JPG)

- 사용자 증가 시 (1)의 과정에서 서버와 데이터베이스를 분리
- 하나는 트래픽을 처리
- 다른 하나는 데이터베이스용

**※ 위와같이 분리함으로써 독립적으로 확장 가능**

### 데이터베이스의 종류와 선택
- 관계형 데이터베이스(RDBMS) : 자료를 열, 칼럼으로 표현, SQL을 사용하면 여러 테이블에 있는 데이터를 그 관계에 따라 조인(join)하여 합칠 수 있다.
- 비 관계형 데이터베이스(NoSQL) : 조인 연산을 지원하지않음
  - 사용목적
    - 아주 낮은 응답 지연시간(latency)이 요구될 때
    - 다루는 데이터가 비정형(unstructured)이라 관계형 데이터가 아닐 때
    - 데이터(JSON, YAML, XML등)를 직렬화하거나(serialize) 역직렬화(deserialize) 할 수 있기만 하면 될 때
    - 아주 많은 양의 데이터를 저장할 필요가 있을 때
  - 분류
    - 키-값 저장소(key-value store)
    - 그래프 저장소(graph store)
    - 칼럼 저장소(column store)
    - 문서 저장소(document store)

## (3) 규모 확장
- 스케일 업(scale up)[수직적 규모 확장] : 서버에 고사양 자원(CPU, RAM등)을 추가, 업그레이드 하는 행위
  - CPU나 메모리를 무한대로 증설할 수 없다는 한계가 있다.
  - 장애에 대한 자동복구(failover) 방안이나 다중화(redundancy) 방안을 제시하지 않는다.
  - 서버에 장애가 발생하면 웹사이트/앱은 완전히 중단된다.
- 스케일 아웃(sacle out)[수평적 규모 확장] : 서버를 추가하는 방식
  - (1),(2)의 경우에서 웹서버가 다운되거나 많은 사용자가 몰리게되면 응답속도가 느려지거나 접속이 불가하게된다.
  - 이 문제를 해결하기 위해 부하 분산기 또는 로드밸런서(load balancer)를 도입한다.

![1.loadbalancer.JPG](../images/1.loadbalancer.JPG)

1. 사용자는 공개 IP(Public IP)로 접속(웹 서버는 클라이언트의 접속을 직접 처리하지 않음)
2. 보안을 위해 서버간 통신에는 사설 IP 주소(Private IP Address)가 이용된다.
   - 사설 IP 주소는 같은 네트워크에 속한 서버 사이의 통신에만 쓰일 수 있는 IP 주소로 인터넷을 통해서 접속할 수 없다.
3. 그림과 같이 서버를 추가하게되면 장애를 자동복구하지 못하는 문제(no failover)는 해소되며, 웹 계층의 가용성(availability)은 향상된다.
   - 서버 1이 다운되면 모든 트래픽은 서버 2로 전송된다. 따라서 웹 사이트 전체가 다운되는 일이 방지된다.
   - 부하를 나누기 위해 서버를 추가 할 수도 있다.
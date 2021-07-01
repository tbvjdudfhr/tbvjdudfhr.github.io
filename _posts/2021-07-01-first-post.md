---
title: "Redis!"
categories:
  - Spring boot
tags:
  - Spring boot
  - Redis
---
- 비 관계형 데이터베이스 시스템(NoSQL)으로, 인메모리 기반이다. key-value 의 맵 형태로 자료를 저장한다.
(캐시, 메시지, 브로커, 키/밸류 스토어 등으로 사용 가능)

# Redis 사용
- Remote Data Store
    - A서버, B서버, C서버에서 데이터를 공유하고 싶을 때
- 한대에서만 필요하다면 전역 변수를 쓰면 되지 않을까?
    - Redis 자체가 싱글 스레드이기에 Atomic을 보장해준다
- 주로 많이 쓰는 곳
    - 인증토큰 등을 저장(String 또는 hash)
    - Ranking 보드로 사용(Sorted Set)
    - 유저 API Limit

# Redis Collections

- Strings 
 List
- Set
- Sorted Set
- Hash

#의존성 추가
Gradle project(build.gradle)
```kotlin
dependencies {
    compile('org.springframework.boot:spring-boot-starter-data-redis')
}
```

# Redis 설치 및 실행(도커)
1. Redis 이미지 다운로드
```shell
docker pull redis
```
2. 컨테이너 생성(기본포트 6379)
docker run --name [컨테이너 이름] -d -p [포트]:6379 redis
```shell
docker run --name redis_test -d -p 6379:6379 redis
```   
3. Redis-cli (Test를 위해)
```shell
docker exec -it redis_test redis-cli
```


[comment]: <> (# 스프링 데이터 Redis)


[comment]: <> (# Redis 주요 커맨드)

[comment]: <> (# Redis 커스터마이징)

[comment]: <> (- spring.redis.*)




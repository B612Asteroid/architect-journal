# Caching

## 존재 이유

- 자주 호출되지만 변경되지 않는 데이터를 DB에서 반복 조회하는 비효율 제거

## 설계 이유

- Redis를 이용, 빠른 응답속도 보장.

## 🛠 적용 예시 / 실전 코드

```java
@Cacheable(key = "'code'")
@CacheEvict(key = "'code'")
@Cacheable(value = "menu", key = "'key:' + #parentId")
// 특정 지점을 주고 싶을때 menu::key:#parentId 형태로 입력
```

## 🧠 아키텍처 개념도

```text
+-------------+        +---------------+        +----------------+
|    Client   | -----> |   Front Store | -----> |   Backend API  |
|             |        |   (Pinia)     |        |   @Cacheable   |
+-------------+        +---------------+        +--------+-------+
                                                        |
                                                        v
                                             +----------------------+
                                             | Redis (TTL 설정됨)   |
                                             +----------------------+
```

## 일기

- 보안 때문에 개고생했다.
- 포트를 열어줄 리 없어서 아예 구현하고 개발서버로 바로 배포 시도 -> 개발서버 여러번 죽음.
- 문제는 Jackson 이었던 모양.

```java
@JsonIgnoreProperties(ignoreUnknown = true)
```

해결

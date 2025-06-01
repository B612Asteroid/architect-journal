# Log 구조화, 고도화

## 존재 이유

- 로그를 구조화

## 설계 이유

- 로그를 구조화 하고, JSON으로 파싱하여 추후 loki, grafana에서 동작하도록 한다.

## 실패 이유

- loki 설치가 안됨.(err="mkdir : no such file or directory\nerror)
- loki, grafana 접근 권한 이슈로 접속 안됨.

## 🛠 적용 예시 / 실전 코드

```java
/**
     * 필요한 이벤트를 log.error 로 남긴다.
     *
     * @param event
     * @param exception
     * @param data
     */
    fun error(
        event: String,
        exception: Throwable,
        data: Map<String, Any?> = emptyMap()
    ) {
        val base = mutableMapOf<String, Any?>(
            "event" to event,
            "traceId" to MDC.get("traceId"),
            "userId" to MDC.get("userId"),
            "timestamp" to Instant.now().toString(),
            "errorType" to exception.javaClass.simpleName,
            "message" to exception.message
        )
        base.putAll(data)
        log.error(base.toString(), extractRootError(exception))

    }

    fun extractRootError(exception: Throwable): String {
        val root = getRootCause(exception)
        val location = root.stackTrace.firstOrNull()?.let {
            "${it.className}.${it.methodName}(${it.fileName}:${it.lineNumber})"
        } ?: "unknown"
        return "${root.javaClass.simpleName}: ${root.message} @ $location"
    }

```

## 일기

- 로그 자체가 이뻐지고, 정확한 로깅만 남는다.
- 원래는 500에러 같은거 stacktrace로 다 나왔는데, 이제는 루트트레이스만 나오니 운영은 보기 쉬워짐.
- 다만 로컬이나 dev는 모든 에러를 보고 추적해야 되는 상황이라, 오히려 역효과였을지도.
- 아, 맞네 profile로 분기하면 되지

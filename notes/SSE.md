# SSE

## 존재 이유

- 서버 측 이벤트 발행. (WS 쓰기는 무겁고, polling은 싫을 때 사용)
- 사용자가 오래 걸릴 수 있는 작업(zip파일 업로드 등)을 비동기로 처리하면서, 사용자에게 긍정적인 피드백을 주기 위해

## 설계 철학

- ZIP 업로드 시 서버 이벤트를 등록.
- ZIP 시작 시 서버에서 이벤트를 발행하여 확인
- 업로드 이후 서버에서 이벤트를 발행하여 토스트 메시지로 띄운다.
- 공통 로직이므로, 사용 언어는 Java를 사용한다.(다른 곳에서도 사용하기 위함.)

## 🛠 적용 예시 / 실전 코드

```typescript
const startEventWatcher = (uploadSessionId: string) => {
  const host = import.meta.env.VITE_API_HOST;
  eventSource = new EventSource(`${host}/sse/upload/${uploadSessionId}`);
  eventSource.addEventListener("extract-zip-event", (event) => {
    const data = JSON.parse(event.data);
  });
  console.log("Received extract-zip event", data);
};
```

```java
public SseEmitter createEmitter(String uploadSessionId) {
        // #. 타임아웃 2분
        var emitter = new SseEmitter(120_000L);
        emitters.put(uploadSessionId, emitter);

        emitter.onCompletion(() -> {
            emitters.remove(uploadSessionId);
        });
        emitter.onTimeout(() -> {
            emitters.remove(uploadSessionId);
            emitter.complete();
        });
}
```

## 일기

- 메시지를 어디서 넣어줘야될까 고민을 많이 했는데, 결국 서버에서 조립하는걸로 함(다국어)
- 토스트를 달았을 때 CSS가 안먹어서 개고생했다.(Primevue CSS를 안넣어줬더라. 아오 퍼블리셔들아...)
- SSE 자체는 엄청 가벼우니, 작은 알림이나 이런 데 사용 여지는 충분히 있다고 생각함.
- 이걸로 채팅기능 만들 생각 한다? 그건 그 사람이 이상한거고.

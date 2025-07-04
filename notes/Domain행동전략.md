# 📌 주제: Domain 행동 전략

## 📅 날짜

2025-05-28

---

## 🧠 요약

도메인이 스스로 어떤 상태를 가지게 하고, 어떤 행동을 취하는지 도메인에 직접 정의한다.

---

## 🔍 개념 설명

- 도메인은 직접 어떤 상태인지, 행동인지를 갖는다.
- 단순 setter가 아닌, 상태 기반 메소드로 정의한다.

---

## 🛠 적용 예시 / 실전 코드

```kotlin
/**
     * 로그인 횟수 초기화
     *
     */
    fun resetLoginAttempt() {
        this.loginCount = 0
    }

    /**
     * 로그인 실패시 로그인 횟수 증가
     *
     */
    fun incrementLoginCount() {
        this.loginCount++
    }
```

## 일기

- 도입에 대해 정말 고민 많이 했다.
- 물론 어떤 도메인이 자신의 행동 양식을 가진다는 것에 대해서는 이견이 없었다.
- 다만, 내가 아닌 어떤 다른 사람에 의해 메소드가 난립한다면 이는 생각해 볼 필요가 있다.
- 이게 DDD의 초입이라는게. 생각보다 재밌다.
- 그리고... 그거 리뷰 다 누가 해... 내가 하잖아.

# 📌 주제: 코틀린 제네릭, 리플렉션

---

## 📅 날짜

2025-05-07

---

## 🧠 요약

Link 데이터 공통 핼퍼 적용 중, 코틀린 제네릭과 리플랙션에 대한 정리.

---

## 🔍 개념 설명

- PersistableLink<S, T>: 관계 추상화 상속 객체
- inline + reified: 컴파일 시점 타입 유지, T::class 사용 가능
- clazz.simpleName vs clazz.kotlin.simpleName: 후자는 reified 필요함에 주의!
- Set의 - 연산: 차집합, invalid = fields - declared
- require(condition) { msg } : 자바의 assert 대체, 명시적 예외 처리

---

## 🛠 적용 예시 / 실전 코드

```kotlin

@MappedSuperclass
abstract class PersistableLink<Source : Persistable, Target : Persistable> protected constructor(
) : Persistable() {
    /* 출발점이 되는 클래스 */
    abstract fun getSource(): Source

    /* 도착점이 되는 클래스 */
    abstract fun getTarget(): Target

    companion object {

        /**
         * Link 객체를 만들 때,
         * Link는 반드시 연결 객체가 모두 있어야만 의미를 가지므로,
         * 반드시 Of로 강제한다.
         *
         * @param source
         * @param target
         * @return
         */
        fun <Source : Persistable, Target : Persistable> of(
            source: Source,
            target: Target
        ): PersistableLink<Source, Target> {
            throw NotImplementedError("하위 클래스에서 반드시 Of를 구현할 것")
        }
    }
}

```

## 일기

- 제네릭을 여기에 쓸지 생각도 못했다. 역시 생각은 열려야 하고, 코드는 계속 봐야 한다.
- 링크만 이렇게 추상화해봐야 사실, 그렇게 크게 의미는 없다. 뭔가 공통화를 위한 코드를 만들어야 할듯.
- 문제는 사용과 전파인데... 과연 사람들이 따라 올려고 할까.

# DDD

## 존재 이유

- 복잡한 비즈니스 로직을 코드에 똑같이 담아내기 위해 사용.
- 기술보다 업무 중심

## 설계 이념

- 비즈니스 언어를 코드 구조로 구현해서 업무와 설계 사이의 거리를 0로 둔다.

## 용어 정리

| 제목                | 내용                                                        |
| ------------------- | ----------------------------------------------------------- |
| 도메인              | 비즈니스 개념 (ex. 주문, 결제, 사용자, 게시글)              |
| 엔티티              | 식별 가능한 객체 (id로 구분됨)                              |
| 밸류 객체 (VO)      | 값으로만 비교되는 객체 (ex. 주소, 이메일)                   |
| 도메인 서비스       | 도메인 간 로직 담당 (ex. 주문 생성 시 포인트 차감)          |
| 애그리거트          | 관련 도메인 묶음의 루트 (ex. 주문 → 주문항목, 결제 등 포함) |
| 레포지토리          | 애그리거트를 저장/조회하는 책임 (ex. 주문 저장소)           |
| 어플리케이션 서비스 | 흐름 조립 역할 (UseCase 역할)                               |

## 🛠 적용 예시 / 실전 코드

```typescript

📦 src
 ┣ 📂 domain
 ┃ ┣ 📂 order
 ┃ ┃ ┣ Order.ts                  ← 엔티티
 ┃ ┃ ┣ OrderItem.ts              ← 밸류 객체 or 서브 엔티티
 ┃ ┃ ┣ OrderService.ts           ← 도메인 서비스
 ┃ ┃ ┣ OrderRepository.ts        ← 인터페이스
 ┃ ┃ ┗ types.ts                  ← 도메인 타입들
 ┃ ┣ 📂 user
 ┃ ┃ ┣ User.ts
 ┃ ┃ ┣ UserProfile.ts
 ┃ ┃ ┣ UserService.ts
 ┃ ┃ ┗ UserRepository.ts
 ┃ ┗ ...
 ┣ 📂 application
 ┃ ┣ 📂 order
 ┃ ┃ ┣ PlaceOrderUseCase.ts      ← 흐름 담당
 ┃ ┃ ┗ CancelOrderUseCase.ts
 ┣ 📂 infrastructure
 ┃ ┣ 📂 order
 ┃ ┃ ┗ OrderRepositoryImpl.ts    ← 실제 DB 연동 구현
 ┃ ┣ 📂 user
 ┃ ┃ ┗ UserRepositoryImpl.ts
 ┣ 📂 presentation
 ┃ ┣ 📂 order
 ┃ ┃ ┗ OrderController.ts
 ┃ ┗ 📂 user
 ┃ ┃ ┗ UserController.ts
 ┗ index.ts (main 진입점)

// 주문 도메인
class Order {
  constructor(private items: OrderItem[], private user: User) {}

  complete() {
    if (this.items.length === 0) throw new Error("빈 주문입니다.");
    this.status = "COMPLETED";
  }
}

// OrderService.ts
class OrderService {
  constructor(
    private orderRepo: OrderRepository,
    private inventory: InventoryService
  ) {}

  placeOrder(userId: string, itemIds: string[]) {
    const order = Order.create(userId, itemIds);
    this.inventory.reserve(itemIds);
    this.orderRepo.save(order);
  }
}
```

## 🧭 전략적 설계 (Strategic Design)
- 어떤 도메인을 누구 팀이 맡을 것인가?
- 팀 간 의존성을 어떻게 설계할 것인가?
- 도메인 간 메시지 흐름(Bounded Context, Context Map)

→ 회사 전체(조직)을 지휘하는 방식에 대한 서술

⸻

## 🧰 전술적 설계 (Tactical Design)
- 도메인을 어떻게 클래스, 폴더, 메서드로 나눌까?
- Entity / VO / Repository / UseCase / Orchestrator 정의
- 흐름 단위의 유닛 테스트 가능한 구조 만들기

→ 이건 일상적인 “코드 설계자”로서의 책임

## 도입하지 않은 이유

- 코드가 많아진다. → 작은 서비스에선 오히려 구조 과잉
- 팀 합의 없이 도입하면 “이게 도메인이야?” 대혼란 발생

## 일기

- 예전에 1년차들로만 만들어진 TF 코드가 이랬음.
- 보니까, 간단한 게시판과 대문인데도 코드가 난잡해서 읽기 어려웠음.
- 물론 DDD의 사상은 정말 마음에 듬.
- 하지만, 팀 규모나 효율 접근 상 아직 안될듯.
- 꼭 적용이 가능한 사이트를 맡아보고 싶다.(물론 야근 많이 하겠지...?)

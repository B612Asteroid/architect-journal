# Nest.js 기반 마이레이션 서버

## 📅 날짜

2025-06-01

## 존재 이유

- 초기 서버 세팅이 빠름
- PM2를 이용한 멀티 인스턴스 기능 제공

## 설계 철학

- 데이터 자체가 빠르게 마이그레이션되어야 함.
- Join 없이 테이블 조회 / 수정이 이루어지므로 @joinColumn 사용하지 않음.
- 스케쥴러에 의한 시간 당 마이그레이션 수행(추후 이벤트 기반으로 확장)
- service는 단순 CRUD만, 실제 실행 자체는 Orchestrator에 위임.

## 🛠 적용 예시 / 실전 코드

```plainText
# 폴더구조
📂 app/
└── 📁 mydomain/
    ├── base/
    │   └── mydomain.base.entity.ts
    ├── source/
    │   └── mydomain.source.entity.ts
    ├── sync/
    │   └── mydomain.sync.entity.ts
    ├── mydomain.module.ts
    ├── mydomain.service.ts
    └── mydomain.controller.ts
```

```typescript

// #. 서비스 클래스 내부 멀티 데이터소스 주입
constructor(
    @Inject("SOURCE_DATASOURCE")
    private readonly sourceDataSource: DataSource,

    @Inject("SYNC_DATASOURCE")
    private readonly syncDataSource: DataSource,
) {}

// #. 서비스 내부
/**
   * 아이템 저장
   * @param manager
   * @param textbooks
   */
  async saveItems(
      manager: EntityManager, // #. 트랜잭션을 위해 엔티티 매니저를 주입
      items: SourceItem[],
  ): Promise<SyncItem[]> {
      const syncItemRepository = manager.getRepository(SyncItem);
      return await syncItemRepository.save(items);
  }

// #. 오케스트레이터에서는 다음과 같이 사용
import { transactional } from "src/utils/transactional";
async migrateAll(): Promise<boolean> {
        this.logger.log("MIGRATE TEXTBOOKS START");
        await transactional(this.syncDataSource, async (manager: EntityManager) => {
        await this.itemService.saveItems(manager, sourceItems);
  })
}
```

## 일기

- 세팅이 간편하다고 해서 Nest.js를 사용했는데 현실은 와르르르
- 트랜젝션을 외부에서 주입한다고???? 맘마미아~~
- 근데 만약 JPA였으면 지금도 세팅중이었을 것. 트레이드오프라 생각하자.

# ServicePart

## 존재 이유

- 비대한 서비스 내 데이터 조각화를 위해 하위 도메인에 부착

## 설계 철학

- ServicePart 내부에서는 실제 저장 로직을 진행한다.
- ServicePart는 Validation 하지 않는다. 모든 데이터가 정제되었다고 판단한다.
- 다른 도메인 간 연계는 ABServicePart에서 진행한다.
-

## 🛠 적용 예시 / 실전 코드

-- ContentsService
ㄴ MetadataServicePart.kt
ㄴ MetadataAttachmentServicePart.kt
ㄴ BoardServicePart.kt 등...

## 일기

- 간단명료함과 SRP 사이에서 고민을 많이 했는데, 이게 최선인듯
- 클래스 갯수가 너무 많아졌다. 이거 어떻게 관리할지도 고민해야함.
- 서비스 클래스 라인 수를 절반으로 줄였으니 나름 괜찮은 성공일지도 모르겠다.

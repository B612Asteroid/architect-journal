# 📌 주제: [여기에 기술 키워드 혹은 개념 입력]

## Presigned-url 발급

## 📅 날짜

2025-05-06

---

## 🧠 요약

S3 Presigned URL 적용

---

## 🔍 개념 설명

- 파일 업로드 / 객체 업로드 간 로직 분리.

---

## 🛠 적용 예시 / 실전 코드

```java
// 예시: Presigned URL 생성
GeneratePresignedUrlRequest request = new GeneratePresignedUrlRequest(...);
URL url = s3client.generatePresignedUrl(request);
```

## 일기

- 이 자체로 일단 업로드는 엄청 빨라짐.
- MultiPart가 너무 무겁다는 생각을 했는데, 이정도면 성공일듯.
- 문제는 ZIP을 어떻게 해야 하나...

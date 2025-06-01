# 📌 주제: IaC

---

## 📅 날짜

2025-05-29

---

## 🧠 요약

- Infrastructure as Code
- 인프 구성을 코드로 선언하고, 버전 관리, 자동화 한다.

---

## 🔍 개념 설명

- IaC: 인프라 리소스를 코드로 정의하여 자동으로 관리
- 불변 인프라: 기존 서버를 수정하지 않고 새로 만드는 방식 (immutable infra)
- 선언형(Declarative): 어떻게가 아닌 무엇을 만들 것인지만 기술
- 버전 관리: Git 등으로 인프라 상태도 기록하고 이력 추적 가능
- CI/CD와 연동: 코드 푸시 → 인프라 자동 배포 가능

---

## 🛠 적용 예시 / 실전 코드

```HCL
provider "aws" {
  region = "ap-northeast-2"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-awesome-bucket"
  acl    = "private"
}

```

## 일기

- 진짜 신기하다. 이게 된다고?
- 다만 사용 효용성은 잘 모르겠다. 이미 올라가있는 인프라를 굳이 내리고 올릴 필요는 없지 않나?

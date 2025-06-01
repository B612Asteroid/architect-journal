# 📌 주제: Axios 커스터마이징

## 📅 날짜

2025-05-27

---

## 🧠 요약

API 요청 공통화를 위해 axios 커스터마이징을 진행한다.

---

## 🔍 개념 설명

- API 요청 공통화
- Axios 사용 시 타입 지정
- querystring 주입
- interceptors를 통한 request/response 처리

---

## 🛠 적용 예시 / 실전 코드

```typescript
const instance = axios.create({
  baseURL: import.meta.env.VITE_API_HOST,
  // timeout: 20000,
  paramsSerializer: (params) => {
    return qs.stringify(params);
  },
});

instance.interceptors.response.use(
  // #. fulfilled
  (response) => {
    if (response.status === 200) {
      return response.data;
    }
  }
  // #. rejected
  async (error) => {
    (...)
  }
);
```

## 일기

- 생각보다 설정해줄게 너무 많았다.(401, 403, 500...)
- 토큰 릴레이를 위해서도 사용되었다.
- 설계하는 사람들은 진짜 디테일한것 까지 챙겨야된다. 대표적인게 이 axios

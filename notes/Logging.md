# Logging

## 존재 이유

- 로그를 수집하고 Grafana로 시각화

## 설계 구조

- PromTail: 로그 데이터 수집용
- Loki: 수신한 로그를 파싱
- Grafana: 시각화

## 🛠 적용 예시 / 실전 코드

```yml
version: "3.8"

services:
  loki:
    image: grafana/loki:2.8.2
    container_name: loki
    ports:
      - "3100:3100"
    volumes:
      - ./loki-config.yaml:/etc/loki/loki-config.yaml
      - ./loki-data:/data
    networks:
      - monitoring-net

  grafana:
    image: grafana/grafana:10.2.3
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - ./grafana-data:/var/lib/grafana
    environment:
      - GF_SERVER_ROOT_URL=/grafana/
    depends_on:
      - loki
    networks:
      - monitoring-net

  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - grafana
    networks:
      - monitoring-net
    restart: always

networks:
  monitoring-net:
    driver: bridge
```

## 특이사항

- 보안 상 3000, 3001 포트 안열림.
- nginx로 proxy-pass 방식 사용

## 일기

- 모니터링 연습용으로 붙여보았다.
- ELK도 생각했지만, 지금 프로젝트보다 ELK가 더 클것 같아서 보류
- 개발의 최대 적은 보안이 맞다. 포트 이거저거 다 막혀서 고생함.
- proxy-pass를 고민할 생각을 하다니, 이건 내가 잘한게 맞다.

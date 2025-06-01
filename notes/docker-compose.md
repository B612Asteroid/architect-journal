# docker-compose를 통한 배포 자동화

## 존재 이유

- 도커 이미지 배포를 GIT에서 관리하여, 변경 이력을 추적하기 위함.

## 설계 이유

- sh 파일이 서버에 있는지 실제로 식별 불가.
- Git에서 실제 배포 파일 구조를 확인하기 위함.
- 필요한 profile을 Parameter화 하여 태깅 자동화.

## 🛠 적용 예시 / 실전 코드

```yml
version: "3.8"

services:
  back-end:
    image: ${IMAGE_URL}:${APP_TAG}
    container_name: api-server
    ports:
      - "8081:8081"
    volumes:
      - /etc/localtime:/etc/localtime:ro
    networks:
      - container-bridge
    restart: always
networks:
  container-bridge:
    external: true
```

```groovy
stage("deploy") {
      steps {
          echo "######################### 🚀 START SSH  ########################"
          sh """
              echo APP_TAG=${env.APP_TAG} > .env.deploy
              scp .env.deploy docker-compose.prod.frontend.yml ncloud@${SSH_HOST}:/srv/
              ssh -t ncloud@${SSH_HOST} <<EOF
              cd /srv
              docker-compose --env-file .env.deploy -p frontend -f docker-compose.prod.frontend.yml down
              docker-compose --env-file .env.deploy -p frontend -f docker-compose.prod.frontend.yml up -d --remove-orphans
              exit
              EOF
          """

          echo "######################### 🚀 END SSH ########################"
      }
  }
```

## 일기

- 이력 추적하겠다고 도전했는데 생각보다 할게 너무 많았다.
- 하나 파일 수정할때마다, back, front, 개발 + 운영 파일을 모두 수정해야 해서 개고생함.
- 근데 이렇게 안하면 관리 안될거니까, 어쩔수 없었다...
- 원래 이런거 인프라팀이나, devOps에서 해줘야되는데 옆팀은 해주는데 우린 안해줌
  작은 프로젝트는 맨날 이래서 서럽다.

# Jenkins Build Script

## 존재 이유

- Jenkins 빌드 유발을 script 형태로 통제

## 설계 이유

- 원래 UI 상에 존재하던 Jenkinsfile을 파일로 분리 / GIT상에서 관리가 가능하도록 변경

## 🛠 적용 예시 / 실전 코드

```groovy
stages {
        stage("git-pull") {
            steps {
                echo "🔄 git pull start"
                git branch: "${GIT_BRANCH}", credentialsId: "jenkins", url: "${GIT_URL}"
                echo "🔄 git pull end"
            }
        }
        stage("build") {
            steps {
                echo "######################### 🐳 START DOCKER BUILD START ########################"

                sh "docker builder prune --filter until=24h --force"
                sh "docker build --tag ${IMAGE_TAG} -f ./Dockerfile ."
                sh "cat /home/docker_credential | docker login ${DOCKER_REGISTRY_URL} -u ${DOCKER_USER_ID} --password-stdin"
                sh "docker push ${IMAGE_TAG}"
                echo "######################### 🐳 END DOCKER BUILD END ########################"
            }
        }
        stage("deploy") {
            steps {
                echo "######################### 🚀 START SSH  ########################"
                sh """ssh -t root@${SSH_HOST} <<EOF
                    sh ~/sh/${SH_COMMEND}
                    exit
                    EOF
                """

                echo "######################### 🚀 END SSH ########################"
            }

        }
    }

```

## 일기

- 원래 파이프라인에 그대로 있던 거 가져옴.
- Webhook으로 플러그인 같은거 안쓰고 바로 슬랙으로 알람 보내게 만들어놓음.
- 근데 보내면 뭐함.
- 읽지를 않는데.
- 그리고 오히려 빌드 속도가 느려진걸로 봐서는... 오버스펙이었을지도.

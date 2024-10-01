# jenkinsAutoDistribution
---
# 목표 및 개요
개발 및 테스트 서버에서 JAR 파일이 갱신되면 운영서버에도 자동으로 반영되는 것이 **목표**입니다.</br></br>
개발 및 테스트 서버의 JAR파일은 Jenkins가 현재 레포지토리의 Commit을 감지하면 JAR파일을 다시 빌드하고 바인드 되어있는 경로에 Copy 합니다.</br>
이후, 개발 및 테스트 서버가 JAR파일이 변경됐음을 감지하고 **SCP**를 통해 운영서버에 JAR파일을 전송합니다.</br>
운영서버는 변경된 JAR파일을 **실시간으로 반영**하여 재실행 시킵니다.</br></br>

# 환경 및 설정
### 가상머신
- myserver01
  - 개발 및 테스트 서버</br>
  - Docker에서 Jenkins 컨테이너 실행 중</br>
- myserver02 : 운영서버</br>
- myserver01에서 Docker로 Jenkins 컨테이너를 실행 중</br>
- Jenkins는 현재 레포(jenkinsAutoDistribution)를 Github hook 설정한 상태</br>
- Jenkins
- myserver02는 JAR 파일 실행 중</br></br>

포워딩 8999:8080으로 설정



프로세스는 다음과 같습니다.</br>
1. Github Commit 발생
2. Jenkins가 감지
   1. JAR 파일 빌드
   2. 마운트 되어있는 경로로 COPY
3. myserver01 에서 실행중인 change.sh에 의해 JAR 파일 갱신 감지 및 재실행
   1. scp로 myserver02로 변경된 JAR 파일 전송
4. myserver02 에서 실행중인 change.sh에 의해 JAR 파일 갱신 감지 및 재실행

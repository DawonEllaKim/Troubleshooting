# [산책할개] - 상세페이지가 안보이는 오류 (카카오맵 app key)

# ❗️Problem

### ✨ 산책가자 상세페이지에서 오류가 떠서 하얀 창이 보이는 오류

<image src="./Images/1.png"/>

# ❗️Thinking Process

1. 콘솔 창 확인후 카카오맵 api 관련 오류라는 것을 알게 됨

   <image src="./Images/2.png"/>

2. “카카오맵 app key Unauthorized” 라고 구글링
3. 이럴때는 새로운 app key 적용을 한 후 플랫폼에도 필요한 사이트 도메인을 추가하면 된다고 함
4. 새로운 키 중에서 Javascript key를 복사

   <image src="./Images/3.png"/>

5. localhost:3000과 지금 잠시 사용중인 HTTP로 배포된 사이트, HTTPS로 나중에 배포될 사이트 도메인 셋 다 등록해주었다.

   <image src="./Images/4.png"/>

6. 그 다음에 index.html 파일의 body 에 45-49번째 줄에서 appkey를 업데이트 해주었다.

   <image src="./Images/5.png"/>

# ❗️Solution

### ✨ 상세 페이지가 전혀 보이지 않는 오류 발생 → 카카오맵 오류임을 파악 → app key 재발급 → 플랫폼에서 사이트 도메인 추가 → 카카오맵 Unauthorized 문제 해결

<image src="./Images/6.png"/>

---

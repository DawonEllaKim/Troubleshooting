# My Page = Your Page

## ❗️ Problem

<img src="Image/1.gif">

1. Nav Bar를 통해서 나의 페이지 방문 (👍🏻 정상 - 나의 userInfo, 나의 게시물 잘 들어옴)
2. Nav Bar를 통해 개스타 or 산책 가자로 이동
3. 게시물을 적은 user의 아이디를 클릭해, 그 사람의 my page로 이동
4. 다른 사람의 my page에 나의 userInfo가 보임, 근데 게시물은 그 사람의 게시물이 맞음 (❗️ 오류! 엄청난 오류!)
5. 이 상태에서 Nav Bar를 통해 나의 my page로 이동
6. userPicture에 편집 아이콘이 생기고 로그아웃 버튼이 생기는 거 보니깐 내 페이지임을 잘 인식 (👍🏻 정상 )
7. 근데! 밑에 게시물은 내가 이전에 들렸던 페이지 주인의 게시물 (❗️ 오류! 엄청난 오류!)

## ❗️ I tried

1. 토큰 확인
   - 처음에는 토큰의 문제인 줄 알았다. "토큰을 잘 못 줘서 다른 사람이 나의 정보를 가지고 있는 거 아닐까?"라는 생각으로 백 앤드 분들과 서버 콘솔을 확인했는데, 각자 사람들에게 알맞은 토큰이 주어짐을 확인했다.
2. 오류의 패턴을 파악
   - 이 엄청난 오류가 갑자기 나기 시작한 건, 우리가 최종 발표 때 이리저리 코드를 수정하면서였다. 문제는 오류가 나는 건 알겠는데 이 오류가 사용자가 언제, 어떻게, 무엇을 했을 때 나는 오류인지를 찾을 수가 없었다. 콘솔에 무언가가 찍히거나, 이 에러의 패턴을 조금이라도 알아야지 감을 잡고 파기 시작할 텐데, 우선 이 에러는 패턴을 찾기 쉽지 않았다.
   - 그러다가 이것저것 사용자가 해볼 수 있는 흐름으로 프로젝트를 실행하다가 오류의 패턴을 찾게 됐다! 그게 바로 위에서 gif로 보이는 패턴이었다! 나의 정보를 받아 온후 그게 내 정보와 어찌어찌 합쳐서 짬뽕이 되는 것!

## ❗️ Solved Process

✍🏻 **1. 모든 My Page에서 나의 userInfo가 보임**

<img src="Image/1.png">

→ 이렇게 콘솔 창을 찍게 코드를 짠 후 오류 나는 패턴방식으로 작동을 해보았다.

<img src="Image/2.png">

→ 제일 처음 나의 페이지에 들어갔을 때 아무런 문제 없이 정상 작동 (나의 강아지 : 쿤이👌🏻 , 나의 고유 ID 값 : 6👌🏻 )

<img src="Image/3.png">

→ 개스타를 타고 남의 my page에 갔을 때 오류 발견!
→ 22번 사람의 페이지이기 때문에 userInfo가 22번 사람의 정보가 들어와야 하는데 여전히 6번 사람(나)의 강아지(쿤이)가 들어옴을 확인! Oh No❗️ 또한, 이 상태에서 나의 페이지로 돌아가면 정말 믹스돼서, 나의 정보는 나오는데 게시물은 남의 것이 나오는 엄청난 오류 ❗️

<img src="Image/4.png">

→ 우리가 currentLogInUserId (=userId)를 파람스를 통해서 받아오는데, 혹시 여기가 문제인가? 생각이 돼서 봤는데 파람스는 올바르게 들어온다. 내 페이지에서는 6번으로 남의 페이지에선 그 사람의 Id로!

<img src="Image/5.png">
<img src="Image/6.png">

→ 그래서 우선 디스패치해 주는 곳을 확인!
→ MyPage를 디스패치하는 곳을 보았는데 우리는 현재 userId (현재 로그인 한 사람의 ID, currentLogInUserId)를 서버에 보내주고 있었다. 그러니까 당연히 서버는 나의 userInfo를 어느 페이지에서나 내려주었고, 우리는 모든 사람의 페이지에서 내 정보가 보이는 거였다.
<img src="Image/7.png">
<img src="Image/8.png">

→ 그래서 디스패치하는곳에서 currentPageUserId를 보내주고 리덕스에서는 url에 제대로된 아이디를 넣어줌으로써 모든 페이지에서 내 userInfo가 보임을 고쳤다 🎉  (이제, 각자 페이지에서 각자의 userInfo가 잘 들어옴!)

✍🏻 **2. 나의 My Page에서 남의 게시물이 보임**
<img src="Image/9.png">

→ 해당 컴포넌트에 들어가서 확인해 본 결과 useEffect에서 getMyPostMD(해당 사용자가 쓴 글 모두 불러오기)를 디스패치 해올 때 두 번째 인자가 빈 배열이었음
→ 그래서 처음 렌더링 될 때는 아직 서버에서 정보를 받아오지 못한 상황에서 이전 사람의 게시물을 받아온다고 판단
→ 빈 배열에 userId(여기서는 userId가 현재 페이지의 주인 ID)를 넣어줬더니 모두 다 정상 작동됨.

## 🚀 Conclusion

- 서버에게 잘못된 정보를 전달함을 파악 후 수정
- 렌더링이 잘못 설정되어 있음을 파악 후 수정
- userId라는 변수가 좀 더 일관성 있고 직관적으로 네이밍 되어야 한다고

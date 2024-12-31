---
layout: post
title: 1. Linking with GitHub and Commit & Push
---

이곳에서는 GUI방식으로 깃 레포지토리를 연동하는 방법과 커밋&푸쉬를 하는 방법을 정리하였습니다. 

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;">
  <img src="/사진들/깃허브연동/깃허브연동1.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    1. 먼저 원하는 프로젝트에 마우스 왼쪽 클릭을 하고<br>
       그 후 컨텍스트 메뉴에서 <code style="background-color: #f4f4f4; font-weight: bold;">Team</code>을 누르고<br>
       <code style="background-color: #f4f4f4; font-weight: bold;">Share Project</code>를 누릅니다.
  </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;">
  <img src="/사진들/깃허브연동/깃허브연동2.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    2. 로컬 레포지토리를 설정해야하는데, 없다면 만들어줍니다.<br>
    만들 때 프로젝트이름이랑 같게 만들기를 권장합니다.<br>
    <code style="background-color: #f4f4f4; font-weight: bold;">ex) 프로젝트이름: jinhwan -> 로컬 레포지토리: jinhwan</code>
  </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;">
  <img src="/사진들/깃허브연동/깃허브연동3.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    3. 처음에는 로컬 레포리지토리와 원격 레포지토리(깃허브의 진짜 레포지토리리)가 연결이 안되어 있기 때문에<br>
    연결해줘야 합니다.&nbsp;&nbsp;<code style="background-color: #f4f4f4; font-weight: bold;">+</code>버튼을 누르고 (git add .) <br> <code style="background-color: #f4f4f4; font-weight: bold;">Commit and Push</code>버튼을 누릅니다. (git commit -m "Commit message" & git push orign master)
  </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;"> 
  <img src="/사진들/깃허브연동/깃허브연동4.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    4. URL입력에 깃 레포지토리 주소만 잘 입력하면 나머지는 알아서 다 잘 해줍니다.<br>
    그리고 "Authentication"에서 사용자 인증을 해줘야하는데, 깃허브 닉네임이랑 PAT발급받아서 입력해줍니다.<br>
    PAT 발급 방법을 모르면 <a href="https://hyeon9mak.github.io/github-personal-access-token/" target="_blank" style="color: blue; text-decoration: underline;">여기</a>를 참조하세요.<br><br>
    * 비고: "Store in Secure Store"칸을 체크하면 다음 커밋&푸쉬할 때, 일일히 인증할 필요가 없습니다.<br>
    (git config --global credential.helper store)

  </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;"> 
  <img src="/사진들/깃허브연동/깃허브연동5.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    5. 원하는 branch를 설정하고 (대개는 master 아니면 main)<br>
    <code style="background-color: #f4f4f4; font-weight: bold;">Preview</code>버튼 눌러주면 연동은 끝납니다.<br>
    (git checkout -b main)
    </p>

</div>
<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;"> 
  <img src="/사진들/깃허브연동/깃허브연동6.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    6. <code style="background-color: #f4f4f4; font-weight: bold;">Push</code>을 누르면 드디어 로컬 레포지토리에서 원격 레포지토리로 파일들이 푸쉬가 됩니다.
    </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;"> 
  <img src="/사진들/깃허브연동/깃허브연동7.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    7. 이 화면이 나오면 문제없이 완료된 것 입니다.
    </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;"> 
  <img src="/사진들/깃허브연동/깃허브연동8.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
    8. 연동이 끝난 후에는 프로젝트를 마우스 우클릭해서 <code style="background-color: #f4f4f4; font-weight: bold;">Team</code>버튼을 누르고 <code style="background-color: #f4f4f4; font-weight: bold;">Commit</code>을 누르면<br> 
    다음과 같은 화면이 뜨는 데, 커밋&푸쉬하고 싶은 수정사항들을 선택하고 <code style="background-color: #f4f4f4; font-weight: bold;">+</code>버튼을 눌러서<br> 
    왼쪽에 커밋 메세지 써주고(무조건) 왼쪽 하단에 <code style="background-color: #f4f4f4; font-weight: bold;">Commit & Push</code>버튼 눌러줍니다.
    </p>
</div>

<div style="margin-top: 30px; margin-bottom: 30px;">
  <hr />
</div>

<div style="display: flex; flex-wrap: wrap; align-items: center; margin-top: 20px;"> 
  <img src="/사진들/깃허브연동/깃허브연동9.png" alt="alt text" style="width: 100%; max-width: 300px; height: auto; margin-right: 20px;" />
  <p style="flex: 1; margin: 0;">
   9. 마찬가지로 이 화면 뜨면 문제없이 완료입니다.
    </p>
</div>

 

# github 사용 방법,협업 방법


폴더에  .git 폴더는 로컬 저장소 

git hub는 원격 저장소 (Repository)

github는 기본 브랜치 이름을 main 으로 강조함 

```powershell
git branch -M main
```

기본 브랜치가 main 으로 설정 됨 

# 깃허브 push방법

터미널 

```powershell
git push -u 저장소주소 main
git push -u https://github.com/Coseung/testgithub.git main
```

[https://github.com/Coseung/testgithub.git](https://github.com/Coseung/testgithub.git)

내 리포지토리 주소

### 에러

push 하려는데 자꾸 다음과 같은 코드가 뜬다 

```powershell
PS C:\Users\dream\OneDrive\바탕 화면\git> git push -u https://github.com/Coseung/testgithub.git main
To https://github.com/Coseung/testgithub.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/Coseung/testgithub.git'        
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

검색해보니 push전에 pull 을 해주라는데 

pull 해도 오류가 떳다

```powershell
git pull https://github.com/Coseung/testgithub.git 
From https://github.com/Coseung/testgithub
 * branch            HEAD       -> FETCH_HEAD
fatal: refusing to merge unrelated histories
```

이 경우에도 검색 해보니 

```powershell
git remot -v
```

위 명령어로 원격 저장소를 확인하고 

연결되어있으면 현재 연결된 주소를 해제 후 재연결을 하는 것

나는 연결이 안되었었다.

연결된 경우 

```powershell
git remote remove 원격저장소 이름 (Coseung)
```

연결 삭제 해주고 

```powershell
git remote add origin 깃 주소 
```

다시 연결후 pull 다음 push 하니 잘 되었다

# git 변수 문법 사용

```powershell
git remote add origin 데이터 
```

데이터를 origin 이라는 변수 명에 저장 

보통 여기에 깃허브 주소 를 넣음

<aside>
🧍🏻

**명령어**

-u 

방금 업로드한 주소를 기억하라는 뜻

</aside>

# github 협업

git clone 명령어 

원격 저장소 복제 

```powershell
 git clone 저장소 주소
```

git push 를 하려면 공동 작업자에 등록해야함

**명령어**

-u 

방금 업로드한 주소를 기억하라는 뜻

개발 시작 하려면 git pull 먼저 

## gitpull

gitpull은 git fetch+git merge 

git fetch : 원격저장소 신규commit 가져오는 명령어 

git merge : 브랜치 에 merge

# 협업 시작시 주의

개발자 가 많아지면 원격 저장소에 개인 브랜치 만들고 나중에 합치는게 좋은 습관

브랜치 만들고  

그 브랜치를 푸쉬 하면 원격저장소에 합쳐짐

```powershell
git push origin 브랜치 이름
```

원격 저장소에 merge 하고 싶다면 

1. 깃헙에서 하던가 
2. 프로그램에서 하던가 

1.깃헙에서 하던가 

- New pull request 클릭
- 어떤 브랜치를 머지 할것인지 선택
- 오류가 나면 깃허브에서 코드 수정 후 MERGE

# 깃 허브 merge 주의

프로젝트 커져도, 사람많아도 branch merge 깔끔하게 하고싶으면

- GitFlow
- Github Flow
- Trunk-based
- Gitlab Flow

### Git Flow 전략

1. main
2. develop
3. feature
4. release
5. hotfix

설명

**역할 : 개발 팀장**

0.9 버전으로 쭉 개발해옴 1.0버전에 중요한 신기능 많이 필요

main(0.9버전) 을 develop이라는 브랜치를 만들어 복사본을 만듬 복사본에 신기능 개발 

develop 브랜치에 push 하면 안됨 충돌 많이 나기때문

그 하위에 브랜치를 하나 더 만듬 이름 : feature

여기에 신기능 개발 

develoop 브랜치에 개발된 1.0이 만족 스러울때 

main브랜치에 바로 올리면 안됨 큰일남

release 라는 브랜치 만들어서 테스트 후 main에 합침 

develop 에도 머지 

버그 발견 → develop브랜치에서 수정후 바로 main에 합침 (hotfix)

main → develop → feature → develop→ release→main

### Trunk-based 개발 전략

main브랜치 하나만 운영 

픽스 할때마다 만 feature 브랜치 만들어 merge
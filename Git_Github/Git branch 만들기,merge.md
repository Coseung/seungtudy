# Git branch 만들기,merge

복습 여부: No

branch - 프로젝트 복사본 

복사본에 개발 하고 머지(merge)하는 방식 

브랜치 만드는 코드

```java
git branch 작명
```

브랜치로 이동 

```java
git switch 브랜치 이름 
git switch main
```

메인 브랜치에 합치고 싶다.(merge)

1. 기준이 되는 브랜치로 이동
2. 머지 할 브랜치 입력

```powershell
git switch main
git merge 머지 할 브랜치 이름
```

다른 사람과 각각 다른내용을 개발 하고 merge 했을때는 성공적이게 끝남 

같은 파일을 수정 했다면 충돌 이 일어남 

- 충돌이 일어나면 원하는 코드만 남기고 저장 하고 git add, git commit 하면 됨

### 다양한 merge방법

- **3-way merge**
    - 브랜치 마다 신규 commit 이 있는 경우 새로운 커밋을 하나 만들어서 merge
        
        ![image.png](Git%20branch%20%EB%A7%8C%EB%93%A4%EA%B8%B0,merge%201817b486260f80aa9f0efa10c2af96d1/image.png)
        
- **fast-forward merge**
    - main브랜치에 신규 커밋이 없을때
    - 딱히 합칠게 없어서 신규 브랜치 보고 main브랜치 라고 붙혀줌
    
    ![image.png](Git%20branch%20%EB%A7%8C%EB%93%A4%EA%B8%B0,merge%201817b486260f80aa9f0efa10c2af96d1/image%201.png)
    

자동으로 fast-forward 방식으로 발동 되기 싫으면 

```powershell
git merge --no-ff 브랜치 명
```

브랜치 삭제 코드

- merge가 끝난 브랜치는 보통 삭제함

```powershell
git branch -d
```

### 

- **rebase**
- 쓰는이유
    - 브랜치가 많을때 너무 복잡해지기 때문에
    - 간단하고 짧은 브랜치들은 쓰면 깔끔
- 단점
    - 충돌 많이남

![image.png](Git%20branch%20%EB%A7%8C%EB%93%A4%EA%B8%B0,merge%201817b486260f80aa9f0efa10c2af96d1/image%202.png)

commit으로 옮겨주는 행위입니다.

1. rebase를 이용해서 신규브랜치의 시작점을 main 브랜치 최근 commit으로 옮긴 다음
2. fast-forward merge하는 것입니다.

rebase merge 방법

- 새로운 브랜치 이동
- git rebase 중심 브랜치명
- 중심 브랜치 이동
- git merge 새로운 브랜치명

### squash and merge

```powershell
git merge --squash 새 브랜치
```

3-way merge처럼 선으로 이어주지 않고

새 브랜치에 있던 코드변경사항들이 **main 브랜치로** 붙혀짐 

![image.png](Git%20branch%20%EB%A7%8C%EB%93%A4%EA%B8%B0,merge%201817b486260f80aa9f0efa10c2af96d1/image%203.png)
# git add,commit,diff


git 설치 후 

폴더 하나 생성후 

![image.png](./git%20add,commit,diff/image.png)

git config —global user.email

git config —global [user.name](http://user.name) 

각각 등록 

```java
git init // 코드 의 변경상황 등 추적 가능
git add 파일 이름 // 파일 add하는것.
git commit -m '커밋내용'// 작명된 파일 을 커밋
git
```

이미지 파일은 거의 안바뀜 

git add 로 파일 가져와서 commit 으로 기록 저장소에 저장 

파일이 여러개 가 있을때 

```java
git add . //모든 파일을 스테이징 해줘라 
git add 파일1 파일2 //파일 1,2스테이징
```

깃 상태 확인 

```java
git status // 상태창 보기  
git log --all --oneline  // 기록보기
```

### Commit 하기전 차이점 비교 하는 습관 들이기

차이점 보여줘 코드

```java
git diff
```

j/k - 이동

q - 나가기 

좀더 비주얼 적으로 훌륭한 에디터 로 보는 코드

```java
git difftool
```

h, j, k, l 방향키

:q   /   :qa 종료 

**Vim 에디터가 싫으면** 

![image.png](./git%20add,commit,diff/image%201.png)

요즘은 에디터에 부가 플러그인이 있음 

git graph 같은 플러그인
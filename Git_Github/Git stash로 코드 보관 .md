# Git stash로 코드 보관

복습 여부: No

특정 코드를 잠깐 잘라내서 보관하고 싶을때 

```powershell
git stash //임시공간에 저장
git stash list //보관된 코드 목록 조회
```

임시 공간에 이동됨

- 최근 commit
- 차이점

전부 보관해줌 

stagin안해놓은 새로운 파일은 stash 안됨

코드 다시 불러오기 

```powershell
git stash pop
```

가장 최근에 했던 것을 불러옴
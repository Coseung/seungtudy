# git 복구


많이 쓰지않음 

참고로 알아두고 필요할때 찾아쓰기 

파일 하나 예전으로 돌아가는법 

```powershell
git restore 파일명 
```

최근 커밋 으로 되돌릴 수 있음.

**최근 커밋 말고 그 전에 커밋 한것으로 돌아 갈때.**

git log —oneline 명령어로 봤을때 좌측 이 커밋아이디 

```powershell
git restore --source 커밋아이디 파일명
```

스태이징 한거 복구 가능 

```powershell
git restore --staged 파일명
```

갑자기 과거 commit이 문제를 일으킬때 

![image.png](/Git_Github/git%20복구/image.png)

여기서 d874b2b 을 커밋 취소 하고 싶을때 

git revert 사용하면 됨

```powershell
git revert 커밋아이디 (여러개 가능 띄어쓰기로)
git revert d874b2b
gir revert HEAD(방금 한거 취소(
```

원래 Commit 내용은 취소 되어있고 

새로 생성해줌

과거로 모든거 되돌리기

- 특정 커밋 아이디 가 커밋 될때로 초기화

```powershell
git resert -hard 커밋 아이디
```
![Dto 이미지](./DTOV.png)


## 엔티티, DTO
엔티티(Entity):

엔티티는 데이터베이스의 테이블과 직접 매핑되는 객체로, 비즈니스 로직을 포함할 수 있다. 엔티티는 데이터베이스와의 상호작용을 위해 설계되며, 변경이 발생하면 데이터베이스 스키마에도 영향을 미칠 수 있다.


DTO(Data Transfer Object):

DTO는 계층간 데이터 교환을 위한 객체로, 로직을 포함하지 않고 순수한 데이터만을 가지고 있다. DTO는 특정 엔티티의 일부 데이터나 여러 엔티티의 조합된 데이터를 전달하는 데 사용될 수 있으며, 클라이언트와 서버 간의 통신에 최적화된 구조를 가진다.

---
## 엔티티 사용 시 문제점
>데이터 노출

엔티티를 직접 사용하면 클라이언트에게 불필요한 정보까지 노출될 수 있음.

>결합도 증가

엔티티 변경이 API 스펙에 직접 영향을 미칠 수 있어, 엔티티와 API 간의 강한 결합이 발생한다. 이로 인해 유지보수가 어려워지며, 확장성이 제한된다.


예시)
#### 게시판
```java
@Getter
@AllArgsConstructor
@NoArgsConstructor
public class Board {
    private String boardId;
    private String title;
    private String contents;
    private String fileName;
    private String memberId;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
}

```
#### 회원
```java
@Getter
@AllArgsConstructor
@NoArgsConstructor
public class Member {
    private String memberId;
    private String memberEmail;
    private String password;
    private String phoneNum;
    private LocalDateTime createdMemberAt;
}

```
만약 엔티티가 이런식의 코드 라고 생각 하고 보자면 
게시판에서 클라이언트 에게 보여줄 내용은 
- 제목(title) 
- 내용(content) 
- 작성자 아이디(memberId) 
- 게시글 작성 날짜(createAt) 

정도가 있지않을까요?

근데 여기서 
>만약 개인적으로 게시판상세에 들어가 댓글을 남기려고 할때 


댓글 단 사람의 아이디(memberId) -member
댓글 의 내용 (replycontent) -reply
게시판의 id (boardId) -board
등 

있을것같습니다 그럼 member 엔티티, reply 엔티티 board 엔티티를 불러와 로직을 만든다고 하면 

---

필요없는
- member의 password, email, create_at, update_at
- bard의 title, contents,fileName 등 

클라이언트에게 필요하지 않은 내용들과 데이터들을 가져와 의도치 않게 데이터를 넘겨줄 수 도있고 


#### member 엔티티에 board 컬럼을 하나 추가한다고 하면
저는 단순히 DB 설계를 바꿨을 뿐인데, 회원 정보만 보여주던 화면에도 갑자기 board 데이터가 딸려 들어가게 됩니다.

---

## DTO를 사용했을 때
그래서 이 문제를 해결하기 위해 **데이터를 전달하기 위한 전용 객체(DTO)**를 따로 만듭니다. 엔티티를 그대로 주는 게 아니라, DTO에 클라이언트가 진짜로 필요로 하는 데이터만 골라서 담아주는 것이죠.

위에서 말한 댓글 상황을 DTO로 만들면 이렇게 됩니다.

댓글 정보 조회용 DTO (ReplyResponseDto)

```java
@Getter
@NoArgsConstructor
public class ReplyResponseDto {
    private String replyContent;    // 댓글 내용 (Reply 엔티티)
    private String memberId;        // 작성자 아이디 (Member 엔티티)
    private String boardId;         // 게시글 아이디 (Board 엔티티)
    private LocalDateTime createdAt; // 댓글 작성일

}
```

이렇게 DTO를 만들어서 사용하면, `Member` 엔티티에 있는 `password`나 `phoneNum` 같은 민감한 정보는 아예 이 클래스에 포함되지 않습니다.

내가 실수로 엔티티를 통째로 넘길 일도 없고, 클라이언트는 딱 댓글 내용과 작성자 아이디만 깔끔하게 받을 수 있게 됩니다. 또한, 나중에 `Member` 엔티티에 `address`가 추가되더라도 이 DTO는 수정할 필요가 없으니 유지보수도 훨씬 쉬워집니다.

---
## 마무리
제가 이해한 바로는 엔티티는 데이터베이스의 **테이블과 매핑**되는 객체이고, DTO는 클라이언트의 **요구사항에 맞춘 데이터 전달 객체**라는 점에서 그 역할이 명확히 다릅니다.

결론은 다음과 같습니다.

엔티티를 API 응답으로 직접 반환하면 데이터베이스 테이블의 변경이 API 스펙에 직접적인 영향을 주게 되어 결합도가 높아집니다. 또한, 패스워드나 내부 로직과 관련된 민감한 데이터가 의도치 않게 클라이언트에게 노출될 수 있습니다.

따라서 데이터베이스 설계의 변경이 외부에 영향을 주지 않도록 독립성을 확보하고, 필요한 데이터만 명확하게 선별하여 전달하기 위해서는 엔티티를 직접 사용하지 않고 반드시 DTO를 사용하여 데이터를 분리해야 합니다.





---

[참고 블로그](https://curiousjinan.tistory.com/entry/Spring-JPA-%EC%97%94%ED%8B%B0%ED%8B%B0%EB%A5%BC-DTO%EB%A1%9C-%EB%B0%94%EA%BF%94%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
참고한 블로그를 보고 제가 이해한 것을 정리하였습니다.
# java 기초

유형: 강의
자료: 2%25EC%259E%25A5.pdf
복습 여부: No

# 자바 기초

변수, 함수, if, for 사용법

main 함수 안에 코드를 짜면 됨

src → main → java → ShopApplication

### 변수 만드는 법

타입 변수명 = 값;

### print 하는 법

System.out.println(변수);

### 클래스

함수는 클래스 안에 작성, main 함수 위

Class는 변수, 함수 보관하는 보관함

class Test {
String name = "kim";
}

### 클래스 가져오는 법

var test = new 클래스이름();

[test.name](http://test.name);

자바에서는 클래스를 많이 씀

원본 데이터를 지킬 수 있고 중요한 변수를 지킬 수 있음

### **Constructor 문법**

클래스 안에 클래스 이름과 같은 함수를 만들고

this를 사용하여 파라미터 방법으로 불러옴

class Friend {
String name = "kim";
Friend(String name) {
    [this.name](http://this.name) = name;
}
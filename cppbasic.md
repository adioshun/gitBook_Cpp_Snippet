# Name Space 

`::`로 구분 하여 표기 

using 네임스페이스 : 네임스페이스 없이 사용, `from numpy import *` 와 동일 

---

# Class 

- `.`연산자로 객체의 멤버 접근 : 일반적 방법 

- `->`연산자로 객체의 멤버 접근 : 객체 포인터 : `*` 와 `&` 이용활용  주소 정보 활용 

## Class 선언 / 사용 

```cpp 
// 선언 
class Fish //대문자
{
 ....
}


//인스턴스 생성 

 Fish fish; //소문자 인스턴스명 
   
```

## 생성자와 소멸자 

클래스 내의 별도의 메소드 이다. 
- 생성자 : 클래스와 동일한 이름 
- 소멸자 : `~`클래스와 동일한 이름 


## 클래스의 상속 

부모 클래스 
```cpp
class NPC {
 ...
}
```

자식 클래스 
```cpp
class Grunt : public NPC{


}

```

---

# 템플릿 

**자료형**이 다를 경우 **미지정**하고 추후 지정하도록 템플릿만 만들어 놓은것 

```cpp
//제작자 코드 
template<typename T> 
class CTest
{
private : 
 T data;  // int data가 아니라 T로 데이터 타입 미정의
}

// 사용자 코드 
int main()
{
  CTest<int> a;  //int로 미정의된 데이터 타입 정
  
  return 0;
}
```

## 템플릿 특수화 

특정 자료형일 경우 **특별히 분리해서** 처리 하는것 
- 특ㅇ 형식은 개발자가 직접 정의할테니 별도로 생성하지 말
- eg. int, double의 경우 덧셈처리가 가능하지만 char인 특정 자료형은 strcpy()으로 처리 해야함 
- 이경우 typename을 지정 안함 : `template<  >`


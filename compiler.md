# 컴파일러 

- gcc : c언어 컴파일러 
- g++ : c++언어 컴파일러 

## c++언어 컴파일 하기 

- gcc로 하기 : `-lstdc++`옵션 사용
    - eg. `gcc -o filename filename.cpp -lstdc++`
    - 옵션 없이 실행시 에러 발생 : `clang: error: linker command failed with exit code 1`
- g++로 하기 : `g++ -o filename filename.cpp`




---

# 여러 소스 코드로 된 프로그램 작성 

![](https://i.imgur.com/0uepAhE.png)

```python 
#기존 방식 
$ gcc -c -o main.o main.c #-c 옵션: 링크를 하지 않고 컴파일만
$ gcc -c -o foo.o foo.c
$ gcc -c -o bar.o bar.c

#최종 링크 작업 수행 
$ gcc -o app.out main.o foo.o bar.o
```

## 방법 #1 : [Make & Makefile](https://www.tuwlab.com/27193)

> make : **Makefile**을 작성하여 위 작업을 자동으로 수행 

규칙 
```xml
<Target>: <Dependencies>
    <Recipe>
```
예제 코드 
```python 
app.out: main.o foo.o bar.o
    gcc -o app.out main.o foo.o bar.o
 
main.o: foo.h bar.h main.c
    gcc -c -o main.o main.c
 
foo.o: foo.h foo.c
    gcc -c -o foo.o foo.c
 
bar.o: bar.h bar.c
    gcc -c -o bar.o bar.c
    
############## 또는 ##################
OBJS=main.o foo.o bar.o
TARGET=app.out
 
all: $(TARGET)
  
clean:
    rm -f *.o
    rm -f $(TARGET)
 
$(TARGET): $(OBJS)
    $(CC) -o $@ $(OBJS)
  
main.o: foo.h bar.h main.c
foo.o: foo.h foo.c
bar.o: bar.h bar.c
```

실행 
```
make 
```

## 방법 #2 : [CMake & CMakeLists.txt](https://www.tuwlab.com/ece/27234)

- Makefile을 보다 쉽고 편리하게 작성할 수 있는 툴
- 문제점 : 소스코드를 수정해서 의존성이 바뀔 때마다 Makefile을 수정 해야 함 
- cmake를 실행 하면 `CMakeLists.txt`를 읽어서 `Makefile`을 생성 한다. 

![](https://i.imgur.com/333cDBz.png)
`CMakeLists.txt에서는 중간 생성물인 Object 파일들은 기술할 필요가 없으므로 생략`

## CMakeLists.txt

스크립트 작성 

```
ADD_EXECUTABLE( app.out main.c foo.c bar.c )
```

실행 

```python
cmake CMakeLists.txt  
```


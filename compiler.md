![](https://i.imgur.com/HoX7vbk.png)

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

> cmake를 실행 하면 `CMakeLists.txt`를 읽어서 `Makefile`을 생성 한다. 

- Makefile을 보다 쉽고 편리하게 작성할 수 있는 툴
- 문제점 : 소스코드를 수정해서 의존성이 바뀔 때마다 Makefile을 수정 해야 함 


![](https://i.imgur.com/333cDBz.png)
`CMakeLists.txt에서는 중간 생성물인 Object 파일들은 기술할 필요가 없으므로 생략`

## CMakeLists.txt

스크립트 작성 

```
ADD_EXECUTABLE( app.out main.c foo.c bar.c )
```

실행 

```python
$ cmake CMakeLists.txt  
$ make
```

---

# 에러 처리 

## 1. 패키지 못 찾는 에러처리 

  ```python
# Could not find a package configuration file provided by

  CMake Error at /opt/ros/melodic/share/catkin/cmake/catkinConfig.cmake:83 (find_package):
  Could not find a package configuration file provided by "pcl" with any of
  the following names:

    pclConfig.cmake
    pcl-config.cmake

  Add the installation prefix of "pcl" to CMAKE_PREFIX_PATH or set "pcl_DIR"
  to a directory containing one of the above files.  If "pcl" provides a
  separate development package or SDK, be sure it has been installed.

  ```

### 1.1 패키지 설치 

- A. 의존성 파일 자동 설치 

```python 
$ rosdep install --from-paths src --ignore-src -r -y
```

- B. 관련 파일 검색후 설치 
```python 
$ apt-get install apt-file && apt-file update

$ apt-file search Qt5CoreConfig.cmake
  """
  # review the result
  qtbase5-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
  qtbase5-gles-dev: /usr/lib/x86_64-linux-gnu/cmake/Qt5Core/Qt5CoreConfig.cmake
  """

$ sudo apt install qtbase5-dev
```
> ref [What package do I need to build...](https://askubuntu.com/questions/374755/what-package-do-i-need-to-build-a-qt-5-cmake-application/374775)


### 1.2 설치는 되어 있으나 못 찾는 문제 

- A. CMakeLists 에 지정 

```Python 
locate PCLConfig.cmake #pclConfig.cmake 위치 확인 
vi  CMakeLists 
set (pcl_DIR "/usr/lib/x86_64-linux-gnu/cmake/pcl") #CMakeList.txt의 `find_package(..)`위에
```

> `pclConfig.cmake`대신 `PCLConfig.cmake`이 존재시 소프트 링크 걸어 주기 
> `$sudo ln -s PCLConfig.cmake pclConfig.cmake`


- B. cmake시 지정 

```Python 
locate PCLConfig.cmake
cmake .. -DPCL_DIR=/usr/local/share/pcl-1.7
```


## 2. 헤더 못 찾는 에러처리 

### 2.2 설치는 되어 있으나 못 찾는 문제 

###### fatal error: ros/ros.h: No such file or directory

```python 
vi ~/.bashrc
export catkin_INCLUDE_DIRS="/opt/ros/melodic/include"

vi CMakelists.txt
include_directories(
    ${PCL_INCLUDE_DIRS}
    ${catkin_INCLUDE_DIRS}  ##추가  
    )
```

###### undefined reference to `ros::spin()'

```python 
vi CMakeLists.txt
target_link_libraries(ground 
   ${catkin_LIBRARIES} #추가 
   )
```




### 빌드후 패키지를 못 찾는 문제 

```python 
vi ~/.bashrc
source /opt/ros/melodic/setup.bash
source ~/catkin_ws/devel/setup.bash
source ~/catkin_build/devel/setup.bash  #추가 
```



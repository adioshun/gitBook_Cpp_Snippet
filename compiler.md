# 컴파일러 

- gcc : c언어 컴파일러 
- g++ : c++언어 컴파일러 

## c++언어 컴파일 하기 

- gcc로 하기 : `-lstdc++`옵션 사용
    - eg. `gcc -o filename filename.cpp -lstdc++`
    - 옵션 없이 실행시 에러 발생 : `clang: error: linker command failed with exit code 1`
- g++로 하기 : `g++ -o filename filename.cpp`
# get_next_line

### Prototype

```c
int get_next_line(int fd, char **line);
```

### Turn in files

- get_next_line.c
- get_next_line_utils.c
- get_next_line.h

### Parameters

1. file descriptor for reading
2. the value of what has been read

### Return value

- 1 : A line has been read
- 0 : EOF has been reached
- -1 : An error happend

### External functs.

- read, malloc, free

### Description

- Write a function which returns a line read from a file descriptor, without the newline

## 선행지식

### 1. File descriptor 관련

- [https://docs.oracle.com/cd/E19476-01/821-0505/file-descriptor-requirements.html](https://docs.oracle.com/cd/E19476-01/821-0505/file-descriptor-requirements.html)
- [https://lowlevel.tistory.com/3](https://lowlevel.tistory.com/3)

### 2. Read 함수

```c
ssize_t read(int fd, void *buf, size_t count);
```

### Description

- fd에서 count 만큼 읽어 buf에 저장한다.

### Return value

- 읽은 count 만큼 return. 에러가 발생했을 시 -1을 return한다.

### 3. Static variable

- 지역변수와 전역변수의 성질을 동시에 가지고 있는 변수. 프로그램이 종료될 때 까지 메모리에 남아있기 때문에 gnl 함수가 다시 호출되었을 때 첫 번째 line이 아닌 두 번째 line을 읽을 수 있도록 할 수 있다. 함수 내에서만 사용이 가능하지만 전역변수처럼 사용이 가능하다.
- [https://blog.naver.com/ddck1321/221865835857](https://blog.naver.com/ddck1321/221865835857)

## 목표

- get_next_line 함수를 호출하면 EOF 전 까지 file descriptor에서 한 번에 한 줄을 읽을 수 있어야 한다.
- file로 부터 읽는 경우와 standard input에서도 작동할 수 있어야 한다.
- 컴파일 시에 -D BUFFER_SIZE=XX flag를 사용하여 BUFFER_SIZE를 임의로 변경할 수 있으며 정의되지 않을 땐 적절한 값을 사용해야 한다.
- BUFFER_SIZE가 매우 클 때 (10000000) 일 때에도 정상적으로 작동해야 한다.
- Static variable을 하나만 사용하여 함수 구현하기.

## 구현 방법

1. fd에서 적절한 BUFFER_SIZE만큼 읽어 buf에 저장한다.
2. buf를 static 변수에 backup 해둔다.
3. static 변수에 개행이 있는지 확인 후 없다면 다시 읽는다.
4. static 변수에 개행이 있다면 개행 전 까지 line에 저장하고, 개행 후로는 다시 static변수에 저장한다.


- 처음에 get_next_line 함수를 완성하고 테스트를 돌려봤을 때 첫 한 줄만 출력이 되고 그 다음줄은 출력이 되지 않았다. 원인을 찾아보니 get_next_line 함수를 2회째 호출할 때 부터 backup 변수에 남아있어야 할 문자열이 사라져 있는 것이었다.
- get_next_line 함수에서 find_ret_value함수로 backup 값을 넘겨줄 때, static variable은 해당 함수 안에서만 값을 변경할 수 있기 때문에 다른 함수에서도 값을 변경하기 위해서는 &연산자를 이용하여 주소값을 넘겨주어야 했다.
- main을 만들어서 테스트 결과, subject에서 원하는 결과와 같이 출력은 되었으나, 다른 사람들이 만들어 놓은 테스트를 돌려본 결과, 일부 BUFFER_SIZE와 매우 큰 BUFFER_SIZE에서 오류가 발생하였다.
- BUFFER_SIZE를 정적으로 할당할 경우 매우 큰 수의 경우에는 할당이 불가하여 segfault 오류가 발생할 수 있으므로 동적할당으로 수정하였다.

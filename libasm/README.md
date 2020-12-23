# libasm

## **Introduction**

- An assembly (or assembler) language, often abbreviated asm, is a low-level programming
language for a computer, or other programmable device, in which there is a very strong
(but often not one-to-one) correspondence between the language and the architecture’s
machine code instructions. Each assembly language is specific to a particular computer
architecture. In contrast, most high-level programming languages are generally portable
across multiple architectures but require interpreting or compiling. Assembly language
may also be called symbolic machine code.

## **General instructions**

- Your functions should not quit unexpectedly (segmentation fault, bus error, double
free, etc) apart from undefined behaviors. If this happens, your project will be
considered non functional and you will receive a 0 during the evaluation.
- Your Makefile must at least contain the rules $(NAME), all, clean, fclean and
re. And must recompile/relink only necessary files.
- To turn in bonuses to your project, you must include a rule bonus to your Makefile,
which will add all the various headers, librairies or functions that are forbidden on
the main part of the project. Bonuses must be in a different file _bonus.{c/h}.
Mandatory and bonus part evaluation is done separately
- We encourage you to create test programs for your project even though this work
won’t have to be submitted and won’t be graded. It will give you a chance
to easily test your work and your peers’ work. You will find those tests especially
useful during your defence. Indeed, during defence, you are free to use your tests
and/or the tests of the peer you are evaluating.
- Submit your work to your assigned git repository. Only the work in the git repository
will be graded. If Deepthought is assigned to grade your work, it will be done
after your peer-evaluations. If an error happens in any section of your work during
Deepthought’s grading, the evaluation will stop.
- You must write 64 bits ASM. Beware of the "calling convention".
- You can’t do inline ASM, you must do ’.s’ files.
- You must compile your assembly code with nasm.
- You must use the Intel syntax, not the AT&T.

## **Mandatory Part**

- The library must be called libasm.a.
- You must submit a main that will test your functions and that will compile with
your library to show that it’s functional.
- You must rewrite the following functions in asm:
    - ft_strlen (man 3 strlen)
    - ft_strcpy (man 3 strcpy)
    - ft_strcmp (man 3 strcmp)
    - ft_write (man 2 write)
    - ft_read (man 2 read)
    - ft_strdup (man 3 strdup, you can call to malloc)
- You must check for errors during syscalls and properly set them when needed
- Your code must set the variable errno properly.
- For that, you are allowed to call the extern ___error.

## Bonus Part

- You can rewrite these functions in asm. The linked list function will use the following
structure:

```c
typedef struct	s_list
{
	void					*data;
	struct s_list	*next;
}								t_list;
```

- ft_atoi_base (like the one in the piscine)
- ft_list_push_front (like the one in the piscine)
- ft_list_size (like the one in the piscine)
- ft_list_sort (like the one in the piscine)
- ft_list_remove_if (like the one in the piscine)

---

## 들어가며

> 이 글은 42seoul의 libasm 과제를 해결하기 위한 자료로 작성되었습니다.

참고문서(ryukim) [https://www.notion.so/Libasm-3c94bbc7df234499b012f6ae82b84dc2](https://www.notion.so/Libasm-3c94bbc7df234499b012f6ae82b84dc2)

> 작동 환경 : masOS(Catalina) 10.15.5

---

## 목차

1. nasm 설치

2. Hello World 출력해보기

3. Mendatory Part

4. Bonus Part

---

1. nasm 설치
    - MacOS 에는 기본적으로 `nasm` 이 설치되어 있지 않다. 따라서 우선 `nasm` 을 설치해줘야 한다.
    - MacOS에서 `nasm`의 설치는 `nasm` 공식 홈페이지에서 압축파일을 직접 다운받아 local에 설치하는 방법과 `Homebrew`를 이용하여 설치하는 방법 두 가지가 있다.

    1.1 공식 사이트에서 파일을 다운받아 설치하기

    - NASM([https://www.nasm.us/](https://www.nasm.us/)) 웹사이트에서 최신버전을 확인한다.
    - nasm-x.xx.xx-tar.gz 형태의 최신 버전을 받아서 압축을 해제한다.
    - nasm-x.xx.xx 폴더에서 ./configure 명령어를 입력한다. 해당 명령어는 Makefile을 이용하여 적절한 C 컴파일러를 찾아주는 명령어이다.
    - `make` 명령어를 입력하여 `nasm`을 build 해준다.
    - `make install` 명령어를 입력하여 `nasm`을 설치한 후, man page를 확인하여 설치된 것을 확인한다.

    1.2 Homebrew를 이용하여 설치하기

    - 우선 `ruby -e "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/master/install](https://raw.githubusercontent.com/Homebrew/install/master/install))" < /dev/null 2> /dev/null` 명령어로 `Homebrew`를 설치한다.
    - 그 다음 `brew install nasm` 으로 `nasm`을 설치한다.
        - permission 오류가 발생할 경우, chown 명령어로 해당 경로에 권한을 주어 설치될 수 있도록 한다. ([참고](https://worthpreading.tistory.com/89))
    - `nasm -ver` 명령어를 이용하여 `nasm`의 버전을 확인할 수 있다.
2. Hello World 출력해보기
    - hello_world.s 파일을 만들고 다음과 같이 입력한다.

        ```wasm
        global    start
            section   .text
        start:
            mov       rax, 0x02000004
            mov       rdi, 1
            mov       rsi, message
            mov       rdx, 13
            syscall
            mov       rax, 0x02000001
            xor       rdi, rdi
            syscall 
            section   .data
        message:  
            db        "Hello, World", 10
        ```

    - nasm 을 이용하여 object 파일을 생성해준다.

        ```wasm
        nasm -f macho64 hello_world.s
        ```

    - linker를 이용하여 실행할 수 있는 파일로 만들어준다.

        ```wasm
        ld -macosx_version_min 10.7.0 -o hello_world hello_world.o
        ```

        [-macosx_version_min flag를 넣어주는 이유](https://waintman.tistory.com/53)

    - hello_world 를 실행해보자.

        ```wasm
        ./hello_world
        ```

        ![libasm%20cbf6436ece0d4b5694c55d67675c613a/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Facb506fe-67eb-407f-b712-41279f96fa41%2FUntitled.png?table=block&id=cbf6436e-ce0d-4b56-94c5-5d67675c613a&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    - global _main 변수로 linking할 경우 ld -lSystem flag를 사용해야 linking 가능하다.
3. Mandatory Part

    ```c
    int		ft_strlen(char const *str);
    int		ft_strcmp(char const *s1, char const *s2);
    char		*ft_strcpy(char *dst, char const *src);
    ssize_t		ft_write(int fd, void const *buf, size_t nbyte);
    ssize_t		ft_read(int fd, void *buf, size_t nbyte);
    char		*ft_strdup(char const *s1);
    ```

    - ft_strlen
        - 64bit macOS에서 parameter는 rdi, rsi, rdx, rcx, r8, r9 register를 통해서 전달되며, 그 이상의 parameter는 stack을 통해 전달된다. 따라서, char const *str은 rdi에 전달되어 있다. 관련 자료는 calling convention을 찾아보자. [참고자료](https://stackoverflow.com/questions/2535989/what-are-the-calling-conventions-for-unix-linux-system-calls-and-user-space-f)
        - rax에 저장되어 있는 값은 함수가 끝났을 때 return하는 값이기 때문에 count index를 rax로 사용하였다.
        - strlen 함수는 문자열의 길이를 반환하는 함수이므로, str의 주소를 참조하여 해당 문자가 null인지 아닌지 판단하여 값을 반환하게 된다.
        - assembly에서 값을 비교할 때는 register의 크기에 주의해야 한다. 문자 하나의 data size는 1byte이기 때문에 null을 판단할 때에도 byte 만큼만 비교해야 한다.
    - ft_strcmp
        - assembly에서는 두 operand를 비교할 때, 둘 중 하나의 operand에만 추가 연산이 가능하다. 예를 들어, a와 b를 index 만큼 이동한 위치를 비교한다고 가정할 때, a + index, b + index 와 같은 연산이 불가능하다는 이야기이다.
        - 따라서 위와 같은 연산을 할 경우에는 다른 register에 값을 옮긴 후 옮겨진 값을 이용하여 연산해야 한다.
        - libft에서 고려하지 않았던 확장 아스키코드와의 비교도 가능하기 때문에 unsigned char의 범위 안에 있는 문자는 모두 비교할 수 있어야 한다.
        - 문자의 차이값을 구하는 과정에서 특정 register의 flag에 주의하여야 한다. overflow가 발생했을 경우에 어떻게 처리해야 할지 잘 생각해봐야 한다.
    - ft_strcpy
        - ft_strcmp와 마찬가지로 하나의 register를 temp로 사용하여 src에 있는 값을 복사한 후 복사된 값이 null인지를 체크하여 마지막에 *dest를 return 해주면 된다.
        - 이 함수를 작성하면서 왜 parameter에서 dest가 먼저 전달되는 지 알 수 있었다. rdi와 rsi의 관계가 dest와 src의 관계와 일치한다.
    - ft_write
        - syscall에 대해 이해하고 있다면 함수를 작성하는 것 자체는 어렵지 않다. 그러나, error처리에 대한 부분을 간과하기 쉽다.
        - syscall 후에 error가 발생했다면, 자동적으로 error number를 rax에 return해준다.
        - error가 발생했을 때, 적절한 return value와 errno를 반환해야 하기 때문에 먼저 ___error를 불러오는 방법부터 알아야 한다.
        - ___error 함수를 호출했을 때, return되는 값은 errno의 주소값이다.
        - errno가 제대로 설정되었는지 확인하려면 standard 함수에서의 errno와 비교해보면 된다. 다만 standard 함수에서 errno 출력 후, ft_write 함수를 불러오기 전에 errno를 다시 초기화 해주어야 하는 것을 잊지 말자.
        - [https://opensource.apple.com/source/xnu/xnu-1504.3.12/bsd/kern/syscalls.master](https://opensource.apple.com/source/xnu/xnu-1504.3.12/bsd/kern/syscalls.master) syscall 목록
    - ft_read
        - syscall 해야 하는 코드만 다를 뿐, ft_write와 거의 동일하다.
    - ft_strdup
        - ft_strlen함수로  strlen을 구한 후 + 1만큼을 malloc하여 ft_strcpy를 이용하여 복사해주면 쉽게 해결할 수 있다.
        - 다른 함수를 호출할 때, caller, callee calling convention에 유의해야 한다.
4. Bonus Part

    ```c
    typedef struct	s_list
    {
    	void					*data;
    	struct s_list	*next;
    }								t_list;

    int			ft_atoi_base(char const *str, char const *base);
    void		ft_list_push_front(t_list **begin_list, void *data);
    int			ft_list_size(t_list *begin_list);
    void		ft_list_sort(t_list **begin_list,int (*cmp)());
    void		ft_list_remove_if(t_list **begin_list, void *data_ref, int (*cmp)(), void (*free_fct)(void *));
    ```

    - ft_atoi_base
        - ft_atoi_base는 piscine때 했었던 함수와 동일하게 작성하면 된다.
        - str에 있는 숫자를 base에 맞는 진법으로 변환하여 출력하는 문제이다.
        - str에서 처리해야 할 문자 순서는 whitespace→sign→number순이다.
        - 나머지 규칙은 piscine의 subject를 보고 해당 규칙에 맞게 작성하면 된다.
    - ft_list_push_front
        - assembly에서 list를 구현하기 위해서는 우선 linked list가 어떠한 구조로 이루어져 있는지 이해할 필요가 있다.
        - 구조체에 선언된 data type의 size만큼 메모리를 차지하기 때문에 구조체의 메모리 주소로부터 + datasize만큼 이동한다면 해당 data의 주소를 얻을 수 있다.
        - linked list에서 list의 위치를 조작할 경우에 반드시 이전 list의 주소를 잃어버리지 않도록 기록해두어야 한다.
    - ft_list_size
        - ft_list_size함수는  ft_strlen에서 배열 대신에 list로 바뀌었을 뿐이다.
        - list→next 가 null  인지 확인한다.
    - ft_list_sort
        - ft_list_sort 함수는 piscine 때의 함수와 같은 함수를 assembly로 작성하는 것이다.
        - list를 정렬할 때, 맨 첫 부분, 중간 부분, 마지막 부분으로 나누어서 swap을 진행하였다.
        - swap algorithm은 bubble sort를 사용하였다.
        - 정 안되겠을 땐, C로 작성한 후에 옮겨보자.
    - ft_list_remove_if
        - ft_list_remove_if 함수는  list에서 data와 일치하는 값을 제거 하는 함수이다.
        - 값을 제거한 후 list를 다시 연결해주어야 하기 때문에 제거되는 위치를 고려해주어야 한다.
        - 비교할 datasize에 유의하기

- 유용한 링크

    [https://cs.lmu.edu/~ray/notes/nasmtutorial/](https://cs.lmu.edu/~ray/notes/nasmtutorial/) NASM Tutorial

    [https://www.tutorialspoint.com/assembly_programming/index.htm](https://www.tutorialspoint.com/assembly_programming/index.htm) Assembly tutorial

    [https://waintman.tistory.com/53](https://waintman.tistory.com/53) linker 에서 _main 함수가 없다고 에러가 발생하는 원인

    [https://medium.com/@thisura1998/hello-world-assembly-program-on-macos-mojave-d5d65f0ce7c6](https://medium.com/@thisura1998/hello-world-assembly-program-on-macos-mojave-d5d65f0ce7c6) hello world 출력해보기

    [https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/x64-architecture](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/x64-architecture) 64비트 아키텍쳐에서의 register size

    [https://blog.naver.com/c3dygks/221637596485](https://blog.naver.com/c3dygks/221637596485) 함수 호출 규약(calling convention)

    [https://dhdhfl.tistory.com/142](https://dhdhfl.tistory.com/142) 64bit 환경 함수 호출 규약

    [https://stackoverflow.com/questions/2535989/what-are-the-calling-conventions-for-unix-linux-system-calls-and-user-space-f](https://stackoverflow.com/questions/2535989/what-are-the-calling-conventions-for-unix-linux-system-calls-and-user-space-f) 64bit calling convention

    [https://github.com/gurugio/book_assembly_8086_ko/blob/master/README.md](https://github.com/gurugio/book_assembly_8086_ko/blob/master/README.md) 어셈기초

    [https://duddnr0615k.tistory.com/263](https://duddnr0615k.tistory.com/263) 레지스터 기초 지식

    [https://btyy.tistory.com/entry/SYSTEM-HACKING-어셈블리-call-jmp-명령의-차이-스택-메모리를-이용한-인자-전달-reverseasm-지역변수-사용해서-표현하는-실습](https://btyy.tistory.com/entry/SYSTEM-HACKING-%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC-call-jmp-%EB%AA%85%EB%A0%B9%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EC%8A%A4%ED%83%9D-%EB%A9%94%EB%AA%A8%EB%A6%AC%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%9D%B8%EC%9E%90-%EC%A0%84%EB%8B%AC-reverseasm-%EC%A7%80%EC%97%AD%EB%B3%80%EC%88%98-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%84%9C-%ED%91%9C%ED%98%84%ED%95%98%EB%8A%94-%EC%8B%A4%EC%8A%B5) call 명령어 관련

    [https://tyeolrik.github.io/assembly/2017/02/25/Assembly-Command.html](https://tyeolrik.github.io/assembly/2017/02/25/Assembly-Command.html) 어셈블리 명령어 정리

    [https://blog.notepads.kr/98](https://blog.notepads.kr/98) cmp flag

    [https://opensource.apple.com/source/xnu/xnu-1504.3.12/bsd/kern/syscalls.master](https://opensource.apple.com/source/xnu/xnu-1504.3.12/bsd/kern/syscalls.master) syscall

    [https://blog.naver.com/seriousboy_/70044054702](https://blog.naver.com/seriousboy_/70044054702) caller, callee calling convention

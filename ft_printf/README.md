# ft_printf

### Prototype

```c
int ft_printf(const char *, ...);
```

### Program name

- libftprintf.a

### Turn in files

- *.c, *.h, Makefile

### Makefile

- all, clean, fclean, re, bonus

### External functs.

- malloc, free, write, va_strat, va_arg, va_copy, va_end

### Description

- Write a library that contains ft_printf, a function that will mimic the real printf

### Comment

- libft 사용 가능
- printf를 최대한 똑같이 동작하게 만들기

## 선행지식

### 1. stdarg.h 관련 지식

- [https://ko.wikipedia.org/wiki/Stdarg.h](https://ko.wikipedia.org/wiki/Stdarg.h)

### 2. printf

- [https://en.wikipedia.org/wiki/Printf_format_string](https://en.wikipedia.org/wiki/Printf_format_string)
- [https://www.freebsd.org/cgi/man.cgi?query=printf&apropos=0&sektion=3&manpath=FreeBSD+12.1-RELEASE+and+Ports&arch=default&format=html](https://www.freebsd.org/cgi/man.cgi?query=printf&apropos=0&sektion=3&manpath=FreeBSD+12.1-RELEASE+and+Ports&arch=default&format=html)
- [https://modoocode.com/35#page-heading-4](https://modoocode.com/35#page-heading-4)

## 목표

- printf 함수와 최대한 똑같이 동작하게 만들기

## 구현 방법

1. format에서 받은 문자열을 저장한다.
2. format[i]가 문자열일 경우 그대로 출력한다.
3. format[i]에 % 문자가 있을 때, flag, width, precision 등을 체크한다.
4. 해당 기호에 맞는 출력형식으로 출력한다.
5. 2번으로 돌아가 반복한다.

## 구현해야 하는 인자

flags : '-0.*'

width

precision

type : cspdiuxX%

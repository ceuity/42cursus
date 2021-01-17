# Libft

## C Library 함수 구현해보기

- Part1
    - memset

        ### Prototype

        ```c
        void *memset(void *s, int c, size_t n);
        ```

        ### Description

        - The memset() function fills the first n bytes of the memory area pointed to by s with the constant byte c.

        ### Return Value

        - a pointer to the memory area s.

        ### Comment

        - unsigned char은 8bit 로 표현되기 때문에 s가 int형일 경우, 32bit에 8bit씩 나눠서 표현되어 원하지 않는 값으로 초기화 될 수 있다.
    - bzero

        ### Prototype

        ```c
        void bzero(void *s, size_t n);
        ```

        ### Description

        - The bzero() function sets the first n bytes of the area starting at s to zero (bytes containing \0).

        ### Return Value

        - None.
    - memcpy

        ### Prototype

        ```c
        void *memcpy(void *dest, const void *src, size_t n);
        ```

        ### Description

        - The memcpy() function copies n bytes from memory area src to memory area dest. The memory areas must not overlap. Use memmove(3) if the memory areas do overlap.

        ### Return Value

        - a pointer to dest.

        ### Comment

        - casting에 주의하기
    - memccpy

        ### Prototype

        ```c
        void *memccpy(void *dest, const void *src, int c, size_t n);
        ```

        ### Description

        - The memccpy() function copies no more than n bytes from memory area src to memory area dest, stopping when the character c is found.

        ### Return Value

        - a pointer to the next character in dest after c, or NULL if c was not found in the first n characters of src.

        ### Comment

        - memccpy 함수 내부에서는 unsigned char 형태로 변환되어 처리되기 때문에 메모리 주소도 1byte씩 움직이기 때문에 일치하는 값 다음 1byte 뒤의 주소를 반환
    - memmove

        ### Prototype

        ```c
        void *memmove(void *dest, const void *src, size_t n);
        ```

        ### Description

        - The memmove() function copies n bytes from memory area src to memory area dest. The memory areas may overlap: copying takes place as though the bytes in src are first copied into a temporary array that does not overlap src or dest, and the bytes are then copied from the temporary array to dest.

        ### Return Value

        - a pointer to dest.

        ### Comment

        - memcpy 함수와 다르게 overlap 되지 않고 안전하게 copy가 가능하다. 필자의 경우는 temp변수를 만들어서 복사하였다.
    - memchr

        ### Prototype

        ```c
        void *memchr(const void *s, int c, size_t n);
        ```

        ### Description

        - The memchr() function locates the first occurrence of c (converted to an unsigned char) in string s.

        ### Return Value

        - a pointer to the byte located, or NULL if no such byte exists within n bytes.
    - memcmp

        ### Prototype

        ```c
        int memcmp(const void *s1, const void *s2, size_t n);
        ```

        ### Description

        - The memcmp() function compares the first n bytes (each interpreted as unsigned char) of the memory areas s1 and s2.

        ### Return Value

        - The memcmp() function returns an integer less than, equal to, or greater than zero if the first n bytes of s1 is found, respectively, to be less than, to match, or be greater than the first n bytes of s2.
        - For a nonzero return value, the sign is determined by the sign of the difference between the first pair of bytes (interpreted as unsignedchar) that differ in s1 and s2.
        - If n is zero, the return value is zero.
    - strlen

        ### Prototype

        ```c
        int strlen(const char *s);
        ```

        ### Description

        - The strlen() function calculates the length of the string s, excluding the terminating null byte (\0).

        ### Return Value

        - the number of bytes in the string s.
    - strlcpy

        ### Prototype

        ```c
        size_t strlcpy(char *dest, char *src, size_t size);
        ```

        ### Description

        - The strlcpy() function copies up to size - 1 characters from the NUL-terminated string src to dst, NUL-terminating the result.

        ### Return Value

        - The strlcpy() and strlcat() functions return the total length of the string they tried to create. For strlcpy() that means the length of src.

        ### Comment

        - src의 length를 반환함에 주의
    - strlcat

        ### Prototype

        ```c
        size_t strlcat(char *dest, char *src, size_t size);
        ```

        ### Description

        - The strlcat() function appends the NUL-terminated string src to the end of dst. It will append at most size - strlen(dst) - 1 bytes, NUL-terminating the result.

        ### Return Value

        - The strlcpy() and strlcat() functions return the total length of the string they tried to create. For strlcat() that means the initial length of dst plus the length of src. While this may seem somewhat confusing, it was done to make truncation detection simple.

        ### Comment

        - length가 dest의 size보다 작으면 src+size를 반환하고, size가 dest + 1보다 클 때, dest+src를 반환한다. strlcat은 strlcpy랑은 다르게 마지막에 NULL문자를 고려하기 때문에 복사할 size가 문자열의 총 길이보다 +1 만큼 커야 한다.
    - strchr

        ### Prototype

        ```c
        char *strchr(const char *s, int c);
        ```

        ### Description

        - The strchr() function returns a pointer to the first occurrence of the character c in the string s.

        ### Return Value

        - The strchr() and strrchr() functions return a pointer to the matched character or NULL if the character is not found. The terminating null byte is considered part of the string, so that if c is specified as (\0) these functions return a pointer to the terminator.

        ### Comment

        - strchr()은 NULL값도 c의 요소로 보기 때문에 마지막에 s[i] == c일 경우에 return(s + i)를 해주는 것에 주의
    - strrchr

        Prototype

        ```c
        char *strrchr(const char *s, int c);
        ```

        ### Description

        - The strrchr() function returns a pointer to the last occurrence of the character c in the string s.

        ### Return Value

        - The strchr() and strrchr() functions return a pointer to the matched character or NULL if the character is not found. The terminating null byte is considered part of the string, so that if c is specified as (\0) these functions return a pointer to the terminator.

        ### Comment

        - strrchr()은 NULL값도 c의 요소로 보기 때문에 마지막에 s[i] == c일 경우에 return(s + i)를 해주는 것에 주의

    - strnstr

        ## Prototype

        ```c
        char *strnstr(const char *haystack, const char *needle, size_t len);
        ```

        ### Description

        - The strnstr() function locates the first occurrence of the null-terminated string needle in the string haystack, where not more than len characters are searched.
        - Characters that appear after a `\0' character are not searched. Since the strnstr() function is a FreeBSD specific API, it should only be used when portability is not a concern.

        ### Return Value

        - If needle is an empty string, haystack is returned;
        - if needle occurs nowhere in haystack, NULL is returned;
        - otherwise a pointer to the first character of the first occurrence of needle is returned.

        ### Comment

        - 남은 문자열의 길이가 find의 길이보다 작을 경우, 더 이상 찾을 필요가 없으므로 NULL을 반환
    - strncmp

        ## Prototype

        ```c
        int strncmp(const char *s1, const char *s2, size_t n);
        ```

        ### Description

        - The strncmp() function compares not more than n characters. Because strncmp() is designed for comparing strings rather than binary data, characters that appear after a `\0' character are not compared.

        ### Return Value

        - an integer greater than, equal to, or less than 0, according as the string s1 is greater than, equal to, or less than the string s2.
        - The comparison is done using unsigned characters, so that '\200' is greater than '\0'.
    - atoi

        ## Prototype

        ```c
        int atoi(const char *str);
        ```

        ### Description

        - The atoi() function converts the initial portion of the string pointed to by str to int representation.

        ### Return Value

        - The converted value.

        ### Comment

        - piscine에서 했던 ft_atoi와는 다른 libc의 atoi임을 주의. long long int 범위를 초과할 때 예외처리를 해주어야 한다.
    - isalpha

        ## Prototype

        ```c
        int isalpha(int c);
        ```

        ### Description

        - The isalpha() function tests for any character for which isupper(3) or islower(3) is true.
        - The value of the argument must be representable as an unsigned char or the value of EOF.

        ### Return Value

        - zero if the character tests false and returns non-zero if the character tests true.
    - isdigit

        ## Prototype

        ```c
        int isdigit(int c);
        ```

        ### Description

        - The isdigit() function tests for a decimal digit character.

        ### Return Value

        - return zero if the character tests false and return non-zero if the character tests true.
    - isalnum

        ## Prototype

        ```c
        int isalnum(int c);
        ```

        ### Description

        - The isalnum() function tests for any character for which isalpha(3) or isdigit(3) is true.

        ### Return Value

        - return zero if the character tests false and return non-zero if the character tests true.
    - isascii

        ## Prototype

        ```c
        int isascii(int c);
        ```

        ### Description

        - The isascii() function tests for an ASCII character, which is any character between 0 and octal 0177 inclusive.

        ### Return Value

        - return zero if the character tests false and return non-zero if the character tests true.
    - isprint

        ## Prototype

        ```c
        int isprint(int c);
        ```

        ### Description

        - The isprint() function tests for any printing character, including space (` ').

        ### Return Value

        - return zero if the character tests false and return non-zero if the character tests true.
    - toupper

        ## Prototype

        ```c
        int toupper(int c);
        ```

        ### Description

        - The toupper() function converts a lower-case letter to the corresponding upper-case letter.

        ### Return Value

        - If the argument is a lower-case letter, the toupper() function returns the corresponding upper-case letter if there is one;
        - otherwise, the argument is returned unchanged.
    - tolower

        ## Prototype

        ```c
        int tolower(int c);
        ```

        ### Description

        - The tolower() function converts an upper-case letter to the corresponding lower-case letter.

        ### Return Value

        - If the argument is an upper-case letter, the tolower() function returns the corresponding lower-case letter if there is one;
        - otherwise, the argument is returned unchanged.
    - calloc

        ## Prototype

        ```c
        void *calloc(size_t nmemb, size_t size);
        ```

        ### Description

        - The calloc() function allocates memory for an array of nmemb elements of size bytes each and returns a pointer to the allocated memory. The memory is set to zero. If nmemb or size is 0, then calloc() returns either NULL, or a unique pointer value that can later be successfully passed to free().

        ### Return Value

        - a pointer to the allocated memory that is suitably aligned for any kind of variable. On error, these functions return NULL.
        - NULL may also be returned by a successful call to malloc() with a size of zero, or by a successful call to calloc() with nmemb or size equal to zero.

        ### Comment

        - nmemb 항목은 size의 갯수를 의미한다. calloc은 malloc과 다르게 선언 후 초기화까지 해주는 함수로 이해하면 될 것 같다.
    - strdup

        ## Prototype

        ```c
        char *strdup(const char *s);
        ```

        ### Description

        - The strdup() function returns a pointer to a new string which is a duplicate of the string s. Memory for the new string is obtained with malloc(3), and can be freed with free(3).

        ### Return Value

        - The strdup() function returns a pointer to the duplicated string, or NULL if insufficient memory was available.
- Part2
    - substr

        ## Prototype

        ```c
        char *ft_substr(char const *s, unsigned int start, size_t len);
        ```

        ### Description

        - Allocates (with malloc(3)) and returns a substring
        from the string ’s’.
        - The substring begins at index ’start’ and is of
        maximum size ’len’.

        ### Return Value

        - The substring. NULL if the allocation fails.

        ### Comment

        - 문자열 s의 start index부터 len만큼의 새로운 문자열을 출력하는 함수.
    - strjoin

        ## Prototype

        ```c
        char *ft_strjoin(char const *s1, char const *s2);
        ```

        ### Description

        - Allocates (with malloc(3)) and returns a new string, which is the result of the concatenation of ’s1’ and ’s2’.

        ### Return Value

        - The new string. NULL if the allocation fails.

        ### Comment

        - 그냥 s1과 s2를 연결하는 함수... piscine에서 한 strjoin과는 다르다. s1 == 0 || s2 == 0 일 때 0을 return한다.
    - strtrim

        ## Prototype

        ```c
        char *ft_strtrim(char const *s1, char const *set);
        ```

        ### Description

        - Allocates (with malloc(3)) and returns a copy of ’s1’ with the characters specified in ’set’ removed from the beginning and the end of the string.

        ### Return Value

        - The trimmed string. NULL if the allocation fails.

        ### Comment

        - 문자열 s1의 시작과 끝에서부터 set에 있는 문자열을 지우는 함수. s1[i]가 set에 없을 때 종료.
    - split

        ## Prototype

        ```c
        char **ft_split(char const *s, char c);
        ```

        ### Description

        - Allocates (with malloc(3)) and returns an array of strings obtained by splitting ’s’ using the character ’c’ as a delimiter. The array must be ended by a NULL pointer.

        ### Return Value

        - The array of new strings resulting from the split. NULL if the allocation fails.

        ### Comment

        - piscine 때 했던 것에서 charset만 char로 바꿔주면 해결
        - 인 줄 알았는데 str을 malloc 하는 과정에서도 malloc이 실패할 경우 split 전체를 free해주어야 한다.
        - substr에서 malloc 한 거 까먹고 get_str에서 malloc 한 번 더 해서 memleak으로 KO
    - itoa

        ## Prototype

        ```c
        char *ft_itoa(int n);
        ```

        ### Description

        - Allocates (with malloc(3)) and returns a string representing the integer received as an argument.
        - Negative numbers must be handled.

        ### Return Value

        - The string representing the integer. NULL if the allocation fails.
    - strmapi

        ## Prototype

        ```c
        char *ft_strmapi(char const *s, char (*f)(unsigned int, char));
        ```

        ### Description

        - Applies the function ’f’ to each character of the string ’s’ to create a new string (with malloc(3)) resulting from successive applications of ’f’.

        ### Return Value

        - The string created from the successive applications of ’f’. Returns NULL if the allocation fails.

        ### Comment

        - 이유는 모르겠지만 temp에 malloc후 f(i, s[i])로 temp를 재구성하였다.
    - putchar_fd

        ## Prototype

        ```c
        void ft_putchar_fd(char c, int fd);
        ```

        ### Description

        - Outputs the character ’c’ to the given file descriptor.

        ### Return Value

        - None

        ### Conment

        - write의 첫 번째 인자만 fd로 변경
    - putstr_fd

        ## Prototype

        ```c
        void ft_putstr_fd(char *s, int fd);
        ```

        ### Description

        - Outputs the string ’s’ to the given file descriptor.

        ### Return Value

        - None
    - putendl_fd

        ## Prototype

        ```c
        void ft_putendl_fd(char *s, int fd);
        ```

        ### Description

        - Outputs the string ’s’ to the given file descriptor, followed by a newline.

        ### Return Value

        - None
    - putnbr_fd

        ## Prototype

        ```c
        void ft_putnbr_fd(int n, int fd);
        ```

        ### Description

        - Outputs the integer ’n’ to the given file descriptor.

        ### Return Value

        - None
- Bonus
    - ft_lstnew

        ## Prototype

        ```c
        t_list *ft_lstnew(void *content);
        ```

        ### Description

        - Allocates (with malloc(3)) and returns a new element. The variable ’content’ is initialized with the value of the parameter ’content’.
        - The variable ’next’ is initialized to NULL.

        ### Return Value

        - The new element.
    - ft_lstadd_front

        ## Prototype

        ```c
        void ft_lstadd_front(t_list **lst, t_list *new);
        ```

        ### Description

        - Adds the element ’new’ at the beginning of the list.

        ### Return Value

        - None

        ### Comment

        - new→next를 lst[0]으로 지정하고 lst[0]에 new의 주소를 입력한다.
    - ft_lstsize

        ## Prototype

        ```c
        int ft_lstsize(t_list *lst);
        ```

        ### Description

        - Counts the number of elements in a list.

        ### Return Value

        - Length of the list.
    - ft_lstlast

        ## Prototype

        ```c
        t_list *ft_lstlast(t_list *lst);
        ```

        ### Description

        - Returns the last element of the list.

        ### Return Value

        - Last element of the list.
    - ft_lstadd_back

        ## Prototype

        ```c
        void ft_lstadd_back(t_list **lst, t_list *new);
        ```

        ### Description

        - Adds the element ’new’ at the end of the list.

        ### Return Value

        - None
    - ft_lstdelone

        ## Prototype

        ```c
        void ft_lstdelone(t_list *lst, void (*del)(void *));
        ```

        ### Description

        - Takes as a parameter an element and frees the memory of the element’s content using the function ’del’ given as a parameter and free the element.
        - The memory of ’next’ must not be freed.

        ### Return Value

        - None
    - ft_lstclear

        ## Prototype

        ```c
        void ft_lstclear(t_list **lst, void (*del)(void *));
        ```

        ### Description

        - Deletes and frees the given element and every successor of that element, using the function ’del’ and free(3).
        - Finally, the pointer to the list must be set to NULL.

        ### Return Value

        - None
    - ft_lstiter

        ## Prototype

        ```c
        void ft_lstiter(t_list *lst, void (*f)(void *));
        ```

        ### Description

        - Iterates the list ’lst’ and applies the function ’f’ to the content of each element.

        ### Return Value

        - None
    - ft_lstmap

        ## Prototype

        ```c
        t_list *ft_lstmap(t_list *lst, void *(*f)(void *), void (*del)(void *));
        ```

        ### Description

        - Iterates the list ’lst’ and applies the function ’f’ to the content of each element. Creates a new list resulting of the successive applications of the function ’f’. The ’del’ function is used to delete the content of an element if needed.

        ### Return Value

        - The new list. NULL if the allocation fails.

        ### Comment

        - f 함수를 적용하여 새로운 list를 만들어 반환하는 함수. malloc이 실패했을 때 del 함수를 이용하여 free한다.

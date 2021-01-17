# minishell

## 들어가며

> 이 글은 42seoul의 minishell 과제를 해결하기 위한 자료로 작성되었습니다.

> 작동 환경 : masOS(Catalina) 10.15.5

---

## External Functions 정리 -yepark-

- fork

    ```c
    #include <unistd.h>

    pid_t fork(void);

    반환 값 : 
    - 성공 시 부모 프로세스에서는 자식 프로세스의 PID값을 반환받음 ; PID > 0 이면 부모다
    - 자식 프로세스에서는 0 값을 반환 받음 ;PID == 0 자식이다.
    - 실패 시 음수 값(-1) 반환 ;PID == - 1 실패다.

    설명 : 
    fork 함수를 호출하는 프로세스는 부모 프로세스가 되고 새롭게 생성되는 프로세스는 자식 프로세스가 됩니다. fork 함수에 의해 생성된 자식 프로세스는 부모 프로세스의 메모리를 그대로 복사하여
    가지게 됩니다. fork 함수를 호출했을 때의 프로세스를 복사함.(지역변수 및 전역변수) 그리고 호출 이후 문장을 실행

    프로세스란 "메모리를 차지한 상태에서 실행중인 프로그램"
    모든 프로세스는 Process ID라는 것을 가집니다.
    운영체제로부터 1번을 제외한 2이상의 Process를 할당 받는다.

    예제 :
    #include <stdio.h>
    #include <unistd.h>

    int gval = 10;
    int main()
    {
    pid_t pid
    	int lval = 20;
    	gval++;, lval += 5;

    	pid=fork();// 자식 프로세스를 생성. 메모리를 그대로 가져가니 gval = 11 lbal = 25가 된다.
    	if(pid == 0) // 자식프로세스일 떄 실행
    		gval += 2, lval+= 2
    	else // 부모프로세스 일 때 실행 
    		gval -=2, lval- -= 2;
    }

    참고 :
    https://codetravel.tistory.com/23

    ```

- wait

    ```c
    #include <sys/wait.h>

    pid_t wait(int *statloc);

    매개변수
    statloc :
    status에는 자식 프로세스가 종료될 때의 상태 정보가 저장된다.
    정상 종료시, status의 하위 8비트에는 0이 저장되며, 상위 8비트에는 프로세스가 종료되게 한 exit 함수의 인수가 기록된다.
    비정상 종료시, status의 하위 8비트에는 프로세스를 종료시킨 시그널의 번호가 저장되며, 상위 8비트에는 0이 저장된다.

    반환 값 :
    - 성공 시 프로세스 ID 반환
    - 실패 시 -1 오류
    - 자식 프로세스가 정상 종료 되었을 때와 비정상 종료 되었을 때, wait() 함수의 반환값과 statloc 인자값이 다르게 채워집니다. 비정상 종료를 알아내는 함수 (WIFEXITED) 같은 매크로?로 체크 가능 WTERMSIG, WEXITSTATUS 등등의 매크로도 있다.. 참고 

    설명 : 부모 프로세스가 fork() 함수를 사용하여 자식 프로세스를 생성하였을 때, fork()함수가 리턴되는 시점부터 2개의 프로세스가 동작하게 됩니다. 부모 프로세스가 자식 프로세스의 종료 상태를 얻기 위해서는 wait() 함수를 사용합니다. 다른 말로 표현하면 wait() 함수를 사용하여 자식 프로세스가 종료 될 때까지 기다릴 수 있습니다.

    wait() 함수는 아래와 같이 동작합니다.
    1. 자식 프로세스가 동작 중이면 호출 차단이 차단되기 때문에 상태를 얻어올 때까지 대기
    2. wait() 함수 호출자가 시그널을 받을 때까지 대기
    3. 자식 프로세스가 종료된 상테라면 즉시 호출이 반환되어 상태를 얻음, 이 떄 wait() 함수는 자식 프로세스의 프로세스 ID를 반환 
    4. 자식 프로세스가 없다면 호출이 즉시, 반환되며, 에러값을 반환

    매크로 함수 :
    WIFEXITED(statloc) : 정상 종료 일 때 참
    WEXITSTATUS(statloc) : 정상 종료일 때 하위 8비트 값
    WIFSIGNALED(statloc) : 시그널에 의해 종료했을 때 참
    WTERMSIG(statloc) : 시그널에 의해 종료했을 때 시그널 번호
    WCOREDUMP(statloc) : 코어 파일 발생 여부
    WIFSTOPPED(statloc) : 일시 중단 상태일 때 참
    WSTOPSIG(statloc) : 일시 중단시킨 시그널 번호

    참고 :
    https://codetravel.tistory.com/30

    ```

- waitpid

    ```c
    #include <sys/wait.h>

    pid_t waitpid(pid_t pid, int *statloc, int options);

    매개변수
    pid :
    pid에 -1 이면 부모 프로세스의 여러 자식 프로세스들 중 어느 한가지라도 종료되는 것을 기다린다.
    pid > 0 일 경우 프로세스 ID가 pid인 자식 프로세스를 기다림
    pid < -1 보다 작을 경우 프로세스 그룹 ID가 pid의 절댓값과 같은 자식 프로세스를 기다림
    pid가 0일 경우 waitpid를 호출한 프로세스의 프로세스 그룹 PID와 같은 프로세스 그룹 ID를 가진 프로세스를 기다림

    statloc : 
    자식 프로세스가 정상적으로 종료 됬을 때 status WIFEXITED(statloc)매크로가 true를 반환, 하위 8비트를 참고하여 자식 프로세스가 exit, _exit, _Exit에 넘겨준 인자값을 얻을 수 있음
    자식 프로세스가 비정상적으로 종료 됬을 때 WIFSGNALED(statloc)매크로가 true를 반환, 비정상 종류 이유를 WTERMSIG(statloc) 매크로를 사용하여 구할 수 있음
    waitpid 함수 오류일 때 ECHILD 호출자의 자식 프로세스가 없는 경우, EINTR : 시스템 콜이 인터럽트 되었을 때

    options :
    WCONTINUED 중단 되었다가 재개된 자식 프로세스의 상태를 받음
    WNOHANG 기다리는 PID가 종료되지 않아서 즉시 종료 상태를 회수 할 수 없는 상황에서 호출자는 차단되지 않고 반환값으로 0을 받음
    WUNTRACED 중단된 자식 프로세스의 상태를 받음
    0을 넣으면 wait()함수와 동일한 동작을 함

    반환 값
    waitpid 함수 오류 일 때 -1 (존재하지 않는 pid넣으면) 리턴 이외는 프로세스ID 리턴

    설명 : waitpid 함수는 wait 함수처럼 자식 프로세스를 기다릴 때 사용하는 함수이다. 즉, 자식 프로세스의 종료상태를 회수할 때 사용한다. 하지만 waitpid 함수는 자식 프로세스가 종료될 때 까지 차단되는 것을 원하지 않을 경우, 옵션을 사용하여 차단을 방지할 수 있습니다.

    ```

- wait3, wait4

    ```c
    #include <sys/wait.h>
    #include <sys/types.h>
    #include <sys/resource.h>

    pid_t wait3(int *statloc, int options, struct rusage *rusage);
    pid_t wait4(pid_t pid, int *statloc, int options, struct rusage *rusage);

    매개변수
    pid : 
    pid < -1 프로세서 그룹 ID가 pid의 절대 값과 같은 어떤 자식 프로세스를 기다린다
    pid == -1 아무 자식 프로세스를 기다린다 wait3과 동일
    pid == 0 프로세스 그룹 ID가 호출 프로세스의 ID와 같은 어떤 자식 프로세스를 기다림
    pid > 0 프로세스 ID가 pid의 값과 같은 자식을 기다림

    statloc 자식 프로세스의 종료 상태를 나타내는 정보를 담아줍니다. 자세한 내용은 wait() 에 있습니다. 
    options 프로세스의 종료 상태 체크 시 옵션을 걸 수 있습니다. wait() 참조 
    rusage 자식 프로세스의 리소스 사용량에 대한 정보가 담깁니다. 
    pid wait4() 에서 사용되는 매개변수로, pid 에 해당하는 프로세스만을 기다립니다.

    반환값
    성공 시 프로세스 ID, 에러 시 -1 리턴
    함수가 WNOHANG 옵션으로 실행되었고, 자식 프로세스가 아직 종료되지 않았을 때 0 리턴

    설명
    wait3 함수는 자식이 종료될 때까지나 현재 프로세스를 종료시키거나 시그널 처리 함수를 호출하는 행동을 하는 신호가 전달될 때까지 현재 프로세스의 실행을 일시 중지한다. 만일 자식이 호출 시간에 이미 종료되었다면 함수는 즉시 리턴한다. 자식이 사용한 시스템 자원들은 모두 풀어진다.

    wait4 함수는 pid인자가 가리키는 자식이 종료될때까지나 현재 프로세스를 종료시키거나 시그널 처리 함수를 호출하는 행동을 하는 신호가 전달될 때까지 현재 프로세스의 실행을 일시 중지된다. 만일 자식이 호출 시간에 이미 종료되었다면 함수는 즉시 리턴한다.
    ㅇㅇㅇㅇ
    사용자 시간 정보를 확인할 수 있습니다. 

    ```

- signal

    ```c
    #include <signal.h>

    void (*signal(int signum, void (*handler)(int)) )(int)

    매개변수

    handler :
    SIG_DFL 기존 방법을 따른다.
    SIG_IGN 시그널을 무시한다.
    함수 시그널이 발생하면 지정된 함수를 호출한다.

    설명
    signal() 함수를 사용하여 프로그램이 운영 체제나 raise() 함수에서 인터럽트 신호를 처리할 수 여러 
    방법 중 하나를 선택할 수 있습니다. SYSIFCOPT(*ASYNCSIGNAL) 옵션으로 컴파일하는 경우 이 함수는
     비동기 신호를 사용합니다.
    ```

- kill

    ```c
    #include <sys/types.h>
    #include <signal.h>

    int kill(pid_t pid, int sig);

    pid 프로세스에 시그널을 보낸다

    반환값
    pid > 0 : pid에 대응되는 프로세스에 시그널을 보낸다.
    pid == 0 : 현재프로세스에 속한 모든 그룹의 프로세스에 시그널을 보낸다.
    pid == -1 : 1번 프로세스 (init) 를 제외한 모든 프로세스에 시그널을 보낸다.
    pid < -1 : pid의 모든 그룹의 프로세스에 signal을 보낸다.

    예시
    int main(int argc, char **argv)
    {
        int pid;
        int sig_num;

        if (argc != 3)
        {
            printf("usage %s [pid] [signum]\n", argv[0]);
            return 1;
        }
        // 실행인자로 pid 번호와 
        // 전송할 signal 번호를 받아들여서 
        // 이를 해당 pid 로 보낸다. 
        pid = atoi(argv[1]);
        sig_num = atoi(argv[2]);
        if(!kill(pid, sig_num))
        {
            perror("Signal Send error");
            return 1;
        }
        return 0;
    }

    https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch06_Signal
    걍이거봐
    ```

- exit

    ```c
    #include <stdlib.h>

    void exit(int status)

    매개변수
    status : 
    0 - 255이거나 매크로 EXIT_SUCCESS 또는 EXIT_FAILURE 중 하나입니다.
    status 값 EXIT_SUCCESS 또는 0은 정상 종료를 표시합니다. 그렇지 않으면 다른 상태
    값이 리턴 됩니다.

    설명
    exit() 함수는 프로그램에서 호스트 환경에 제어를 리턴합니다. atexit() 함수에 등록된
    모든 함수를 역순으로 호출합니다. 프로그램을 종료하기 전에 버퍼를 모두 삭제하고 
    열린 파일을 모두 닫습니다.
    ```

- getcwd

    ```c
    #include <unistd.h>

    char *getcwd( char *buffer, int maxlen );
    //buffer : 파일의 경로
    //maxlen : 파일에 설정할 접근 모드 값

    매개변수

    buf 현재 디렉토리의 위치를 알려주는 명령은 pwd다. 
    프로그램에서 디렉토리의 위치를 알아내는 데는 getcwd함수를 이용한다. 
    getcwd 함수는 현재 디렉토리의 절대 경로를 저장할 버퍼의 주소와 버퍼의 크기를 인자로 받는다. 
    만약 버퍼의 주소가 널이면, getcwd는 직접 malloc으로 메모리를 할당하고 주소를 리턴한다. 
    getcwd는 수행 도중 오류가 발생하면 널을 리턴한다. 

    ```

- chdir

    ```c
    #include <unistd.h>

    int chdir(const char *dirname);

    현재 디렉토리를 dirname가 지정한 곳으로 바꾼다.

    성공 시 0 실패시 -1 리턴

    변수에 . 이랑 .. 넣으면 알아서 해주는 역할임 ㅎ
    ```

- stat

    ```c
    #include <sys/types.h>
    #include <sys/stat.h>
    #include <unistd.h>

    int stat(const char *path, struct stat *buf);
    파일의 크기, 파일의 권한, 파일의 생성일시, 파일의 최종 변경일 등, 파일의 상태나 파일이 정보를
    얻는 함수입니다. stat(2) 함수는 symbolic link인 파일을 path로 넘기면 그 원본 파일의 정보를
    얻습니다. lstat(2)함수는 symbolic link 파일 자신의 정보를 얻습니다. 나머지 기능은 stat(2)와
    lstat(2)는 같습니다.

    헤더 : stat.h types.h unistd.h
    매개변수 : path = 절대경로로 줘야함. buf = stat 구조체 주소
    반환값 : 성공 = 0, 실패 = -1

    이 함수는 우리가 가장 많이 사용하는 ls -al 명령어로 알 수 있는 내용들을 대부분 알 수 있습니다.

    struct stat { 
    dev_t st_dev; /* ID of device containing file */ 
    ino_t st_ino; /* inode number */ 
    mode_t st_mode; /* 파일의 종류 및 접근권한 */ 
    nlink_t st_nlink; /* hardlink 된 횟수 */ 
    uid_t st_uid; /* 파일의 owner */ 
    gid_t st_gid; /* group ID of owner */ 
    dev_t st_rdev; /* device ID (if special file) */ 
    off_t st_size; /* 파일의 크기(bytes) */ 
    blksize_t st_blksize; /* blocksize for file system I/O */ 
    blkcnt_t st_blocks; /* number of 512B blocks allocated */ 
    time_t st_atime; /* time of last access */ 
    time_t st_mtime; /* time of last modification */ 
    time_t st_ctime; /* time of last status change */ 
    };

    https://www.it-note.kr/173 //여기 is를 통한 디렉토리인지 일반파일인지 확인하는거 있음

    ```

- lstat

    ```c
    #include <sys/types.h>
    #include <sys/stat.h>
    #include <unistd.h>

    int lstat(const char *path, struct stat *buf);

    lstat함수는 기존의 stat함수처럼 stat구조체를 채우는것은 동일
    lstat함수는 path가 심볼릭 링크 파일일 경우, 심볼릭 링크파일에 대한 정보를 구조체에 채운다.
    (stat함수는 원본의 정보를 채운다.)
    ```

- fstat

    ```c
    #include <sys/types.h>
    #include <sys/stat.h>
    #include <unistd.h>

    int fstat(int fd, struct stat *buf);

    stat, lstat와 첫 번째 인자가 다르다.
    fstat함수는 첫번째 인자로 파일 디스크럽터 번호를 인자로 받고 stat와 동일한 기능을 수행한다.

    ```

- execve

    ```c
    #include <unistd.h>

    int execve(const char *filename, char *const argv[], char *const envp[]);
    실행가능한 파일인 filename의 실행코드를 현재 프로세스에 적재하여 기존의 실행코드와 교체하여
    새로운 기능으로 실행합니다. 즉, 실행되는 프로그램의 기능은 없어지고 filename 프로그램을
    메모리에 loading하여 처음부터 실행합니다.

    execve 호출 후에 일어나는 프로세스의 변화입니다.

    - signal 설정이 default로 변경됩니다.
    - mmap(2)으로 생성 memory mapping이 보존되지 않습니다.
    - shared memory 영역에 대한 access가 해제됩니다.
    - message queue descriptor가 close됩니다.
    - named semaphore가 close됩니다.
    - timer가 보존되지 않습니다.
    - opendir로 open된 directory stream이 close됩니다.
    - memory lock이 보존되지 않습니다.
    - atexit(3), on_exit(3)로 등록된 exit handler가 해제됩니다.
    - 모든 thread가 사라집니다.
    - aio_read(3)/aio_write(3)과 같은 async I/O 동작이 취소됩니다.
    - 일반 file descriptor는 close되지 않습니다.

    exec 뒤에 붙는 글자의 의미
    l : argv가 list로 나열된다는 의미입니다. 그 것의 끝은 NULL. 
    v : argv가 vector(배열)로 parameter를 하나를 받는다는 의미입니다. 
    		배열의 마지막값은 NULL 
    p : 첫번째 파라미터인 명령어/실행파일이 PATH로 지정된 디렉토리에 있다면 full path 
    		또는 상대 path로 하지 않아도 된다는 뜻입니다. 
    e : 설정할 환경변수를 parameter로 받는다는 의미입니다.

    출처에 더 자세한 내용이.!!
    출처: https://www.it-note.kr/157 [IT 개발자 Note]
    ```

- dup

    ```c
    #include <unistd.h>

    int dup(int fd);

    dup는 fd로 전달받은 fd를 복제하여 반환합니다. dup가 돌려주는 fd는 가장 낮은
    서술자를 반환합니다. 성공시 새로운 파일디스크럽터 반환 실패시 -1\
    복제다.. 파일디스크럽터를 복제해주는거다!!!!

    //?? fd를 그대로 복사하는건가 뭐고 ;;!
    ```

- dup2

    ```c
    #include <unistd.h>

    int dup2(int fd, int fd2);

    dup2는 새 서술자의 값을 fd2로 지정합니다. 만일 fd2가 이미열려있으면 fd2를 닫은 후 복제가
    됩니다. 역시 성공시 새 파일 서술자, 오류시 -1을 반환합니다.

    ps. 듑2 나븐새기입니다 ㄹㅇ; 

    쉽게말하면 리턴 값은 fd가 가르키는 것을 가지는데 그 값은 fd2로해라 입니다.
    결국 fd2도 fd가 가르키는 것을 가르키게 되죠..

    ```

- pipe

    ```c
    #include <unistd.h>
    int pipe(int fd[2]);

    반환값 성공시 0, 실패시 -1

    파이프 생성 함수
    - 하나의 파이프 및 파이프에 대한 두 개의 파일 디스크럽터가 생성
    - 하나의 파이프를 프로세서들이 공유

    역할 프로세스는 메모리가 독립적으로 존재하기 때문에 메모리를 프로세스간 데이터를 주고 받는 것은 
    불가능 합니다. 쉽게 설명하면 fork()이용 하여 server 혹은 client 쪽에서 생성된 부모 자식
    프로세스 들은 프로그램내 프로세스끼리는 데이터를 주고 받을 방법이 없습니다.
    해결책 !!! : 서로 독립된 프로세스들이 데이터를 주고 받기 위해 운영체제는 '파이프'라는 함수를
    제공하고 있습니다.

    fd: 크기가 int형 배열을 요구
    fd[0] : 함수 호출 후 fd[0]에 데이터를 입력 받을 수 있는 파일 디스크럽터가 담김(파이프 출구)
    fd[1] : 함수 호출 후 데이터를 출력할 수 있는 파일 디스크럽터가 담김(파이프 입구)

    https://smeffect.tistory.com/entry/05-pipe-%ED%8C%8C%EC%9D%B4%ED%94%84
    ```

- opendir

    ```c

    #include <sys/types.h>
    #include <dirent.h>

    DIR *opendir(const char *dirname);
    dirname: 열려는 디렉토리명
    헤더 : dirent.h

    예시
    DIR *dir_info;

    dir_info = opendir("."); //현재 디렉토리를 열기

    ```

- readdir

    ```c
    #include <sys/types.h>
    #include <dirent.h>

    struct dirent *readdir(DIR *drip);
    drip: 정보를 읽어올 디렉토리를 가르키는 포인터

    dirent 구조체는 <sys/dirent.h> 파일에 정의되어 있다.

    readdir()은 인수로 받은 디렉토리에 대해, 함수를 호출할 때 마다 디렉토르 안에 있는 파일이나
    디렉토리명을 반환합니다. 모든 파일과 디렉토리를 읽어 들이면 NULL을 반환하게 되며, 이름 문자열
    struct dirent에서 d_name에 저장됩니다.

    예시
    struct dirent *dir_entry;

    dir_entry = readdir(dir_info) // 디렉토리 안에 있는  모든 파일과 디렉토리 출력

    구조체 정보
    typedef struct dirent{
    ino_t d_ino; // readdir 함수로 읽어온 항목의 inode 번호
    off_t d_off; // 디렉토리 내에 있는 항목의 오프셋
    unsigned short d_reclean; // 디렉토리 항목의 레코드 길이
    char d_name[1]; // 항목의 이름
    }dirent_t
    ```

- closedir

    ```c
    #include <sys/types.h>
    #include <dirent.h>

    int closedir(DIR *drip)
    헤더 dirent.h
    반환 성공 = 0, 실패 = 1
    drip: 닫으려는 디렉토리를 가리키는 포인터
    ```

- strerror

    ```c
    #include <string.h>

    char* strerror(int errno);

    오류 메시지 문자열을 가리키는 포인터를 얻어온다. errno의 값을 통해 발생하였던 오류에 알맞은 
    오류에 알맞은 오류 메시지를 가리키는 포인터를 리턴한다. 이 때 리턴되는 포인터는 문자열 리터럴을
    가리키고 있기 때문에 그 내용이 바뀔 수 없다. 참고로 strerror에 의해 출력되는 오류 메시지는
    현재 사용중인 컴파일러나 플랫폼에 따라 다를 수 있다.

    ```

- errno

    ```c
    #include <errno.h>

    errno : 오류 번호

    정보 : errno 전역변수는 전역변수처럼 보이기 때문에 Multi-thread에 안전하지 않은 것 같지만,
    thread마다 다르게 관리되므로 Multi-thread에 안전합니다.

    주의 : strerror(3)함수는 Multi_thread unsafe한 함수이므로 Multi_thread 프로그램에서
    는 strerror_r(3)을 이용합니다. 
    ```

## 구현해야 할 인자

- echo [-n]

    -n 옵션이 있으면 마지막 줄에 개행을 출력하지 않는다.

- cd [relative or absolute path]

    chdir 함수를 이용하여 경로 변경

    경로를 변경하고 나면 PWD에 getcwd로 현재 경로 갱신

- export

    linked list로 만든 env list에 add_list로 넣어주기

- unset

    env list에서 key 값을 찾아서 제거해주기

- env

    envp 배열에서 =을 기준으로 split하여 key, value 구조체에 넣어주기

    ```c
    typedef struct s_env
    {
        char *key;
        char *value;
        struct s_env *next;
    }              t_env;
    ```

- exit

    argv에 숫자가 아닌 것이 있을 때 numeric argument required 255 반환하면서 종료

    argv가 1개 이상일 때, too many arguments 1 반환 Error

- ;

    tokenize 해서 linked list로 연결한 다음 list→next == NULL  일 때 까지 명령어 실행해주기

    ```c
    typedef struct s_semicolon
    {
        char *cmd;
    		char *flag;
    		void *content;
    		struct s_semicolon *next;
    }              s_semicolon
    ```

- ' and " except for multiline commands

    dquote는 환경변수만, quote는 전부 문자열

- Redirections < > ">>" except for file descriptor aggregation

    opendir, readdir, closedir과 fd 이용해서 리디렉션 해주기

- pipes

    pipe 함수 사용해서 fd 관리

- $(env)

    $(key)로 env에서 strcmp 함수로 일치하는 키를 찾아서 value를 출력

    치환 단계는 세미콜론(;) → 파이프(|) → $(env) 치환 → tokenize

- $? (last errno)

    exit함수의 parameter로 종료코드를 설정할 수 있다.

- ctrl-C, ctrl-D, ctrl-\

    signal로 처리할 수 있다.

    ctrl-C SIGINT

    ctrl-\ SIGQUIT

    ctrl-D EOF

## 용어정리

- 유용한 링크

    [https://stackoverflow.com/questions/33884291/pipes-dup2-and-exec](https://stackoverflow.com/questions/33884291/pipes-dup2-and-exec)

    [https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch05_Process](https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch05_Process)

    [https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch06_Signal](https://www.joinc.co.kr/w/Site/system_programing/Book_LSP/ch06_Signal)

## C로 Shell을 만드는 방법

### Shell의 기본 주기 [참고](https://brennan.io/2015/01/16/write-a-shell-in-c/)

- 초기화 : Shell 구성 파일을 읽고 실행한다.
- 해석 : stdin으로 입력을 받아 parsing 후 command에 맞게 실행한다.
- 종료 : 사용되었던 메모리를 free 해준 후, command를 종료한다.

### Shell의 기본 루프

- Read : standard input으로 명령어 읽기
- Parse : program 과 arguments로 구분하기
- Execute : parsing 된 명령어를 실행하기

## Todo-list
- [x]  구조체 만들기
- [x]  signal
    - [x]  Ctrl-C -> sigint
    - [x]  Ctrl-\ -> sigquit
    - [x]  Ctrl-D -> EOF
- [x]  envp
    - [x]  linked list envp
    - [x]  **evnp
- [x]  명령줄 입력받기
    - [x]  \ -> gnl에서 받은 input의 가장 마지막 문자가 \ 일 때, 개행 후 다시 gnl 입력 받기
    - [x]  quote, dquote
- [x]  tokenize
    - [x]  quote, dquote
    - [x]  space( )
    - [x]  pipe(|)
    - [x]  semicolon(;)
- [x]  builtin command
    - [x]  echo
    - [x]  cd
    - [x]  export
    - [x]  unset
    - [x]  env
    - [x]  exit
    - [x]  $?
- [x]  $(env)
- [x]  pipe
- [x]  semicolon(;)
- [x]  < > >> redirection
- [x]  PATH 파싱 후 binary file execute

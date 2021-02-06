# philosophers

- threads 에 대하여 배우는 과제
- mutex, semaphore

# Mandatory Part

- C로 짜야 하고, norm도 맞춰야 한다. 암튼 터지면 안됨
- 테이블에 둘러앉은 철학자들은 3가지를 한다. 먹고, 생각하고, 자기
- 먹는 동안은 생각하거나 잘 수 없고, 자는 동안엔 먹거나 생각할 수 없고, 생각하는 동안엔 먹거나 잘 수 없다.
- 철학자들은 테이블에 원형으로 앉아있고, 가운데는 큰 스파게티가 있다.
- 몇 개의 포크가 테이블에 있다.
- 스파게티는 포크 하나로 못 먹기 때문에 철학자들은 반드시 한 손에 포크 하나씩 포크 두 개를 사용해야 한다.
- 철학자는 굶어선 안된다.
- 모든 철학자는 먹어야한다.
- 철학자는 다른 철학자와 얘기하지 않는다.
- 철학자는 다른 철학자가 언제 죽을지 모른다.
- 철학자는 식사가 끝날 때 마다 포크를 놓고 잔다.
- 철학자는 자고 일어나면 생각한다.
- 철학자가 죽으면 시뮬레이션이 끝난다.
- 각 프로그램은 같은 옵션을 써야 한다.

    number_of_philosophers : 철학자와 포크의 수

    time_to_die : 단위는 ms이며, 시뮬레이션이 시작하거나 마지막 식사 이후 'time_to_die' 안에 먹는 것을 시작하지 못하면 철학자는 죽는다.

    time_to_eat : 철학자가 먹는데 걸리는 시간이며, 먹는 동안에는 포크 2개를 유지한다.

    time_to_sleep : 철학자가 자는데 쓰는 시간

    number_of_times_each_philosopher_must_eat : 이 argument 는 선택사항이다. 시뮬레이션이 멈추기 위한 최소 식사 횟수 조건. 만약 정의되지 않으면 철학자가 죽을 때 시뮬레이션이 끝난다.

- 각 철학자는 1부터 number_of_philosophers 까지 숫자가 주어진다.
- 1번 철학자 옆에는 number_of_philosophers 숫자의 철학자가 있으며, N번째 철학자 옆에는 N-1과 N+1이 있다.
- 철학자의 어떤 상태 변화든 아래 규칙을 따라 쓰여져야 한다. (X는 철학자 숫자)
    - timestamp_in_ms X has taken a fork
    - timestamp_in_ms X is eating
    - timestamp_in_ms X is sleeping
    - timestamp_in_ms X is thinking
    - timestamp_in_ms X died
- 상태 출력할 때 다른 철학자랑 섞이게 하지 마라
- 철학자의 죽음과 철학자가 죽은 것을 출력할 때 10ms 이상 넘기면 안된다.
- 철학자를 죽이지 마라!

## 함수 정리

- usleep()

    ```c
    #include <unistd.h>
    int usleep(useconds_t useconds);

    microseconds 단위로 함수를 대기시키는 함수
    1ms = 1000us
    ```

- gettimeofday()

    ```c
    #include <sys/time.h>
    int gettimeofday(struct timeval *restrict tp, void *restrict tzp);

    struct timeval start, end;
    double diff;
    gettimeofday(&start, NULL);
    usleep(1000)
    gettimeofday(&end, NULL);
    diff = (end.tv_sec - start.tv_sec) * 1000.0 + (end.tv_usec - start.tv_usec) / 1000.0
    printf("%f\n", diff);

    ```

- pthread [https://reakwon.tistory.com/56](https://reakwon.tistory.com/56)
    - pthread_create

        ```c
        int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void*), void *arg)

        1. thread : 성공적으로 함수가 호출되면 이곳에 thread ID가 저장됩니다. 이 인자로 넘어온 값을 통해서 pthread_join과 같은 함수를 사용할 수 있습니다.
        2. attr : 스레드의 특성을 정의합니다. 기본적으로 NULL을 지정합니다. 만약 스레드의 속성을 지정하려고 한다면 pthread_attr_init등의 함수로 초기화해야합니다.
        3. start_routine : 어떤 로직을 할지 함수 포인터를 매개변수로 받습니다. 
        4. arg : start_routine에 전달될 인자를 말합니다. start_routine에서 이 인자를 변환하여 사용합니다.
        ```

    - pthread_join

        ```c
        int pthread_join(pthread_t thread, void **retval)

        1. thread : 우리가 join하려고 하는 thread를 명시해줍니다. pthread_create에서 첫번째 인자가 있었죠? 그 스레드가 join하길 원한다면 이 인자로 넘겨주면 됩니다.
        2. retval : pthread_create에서 start_routine이 반환하는 반환값을 여기에 저장합니다.
        ```

    - pthread_detach

        때에 따라서는 스레드가 독립적으로 동작하길 원할 수도 있습니다. 단지 pthread_create 후에 pthread_join으로 기다리지 않구요. 나는 기다려주지 않으니 끝나면 알아서 끝내도록 하라는 방식입니다.

        독립적인 동작을 하는 대신에 스레드가 끝이나면 **반드시 자원을 반환**시켜야합니다. pthread_create만으로 스레드를 생성하면 루틴이 끝나서도 자원이 반환되지 않습니다. 그러한 문제점을 해결해주는 함수가 바로 pthread_detach입니다.

        ```c
        int pthread_detach(pthread_t thread)

        thread는 우리가 detach 시킬 스레드입니다. 
        성공시 0을 반환하며 실패시 오류 넘버를 반환하지요.
        pthread_detach와 pthread_join을 동시에 사용할 수는 없습니다.
        ```

- mutex [https://reakwon.tistory.com/98](https://reakwon.tistory.com/98)
    - pthread_mutex_init

        1) 정적으로 할당된 뮤텍스를 초기화하려면 **PTHREAD_MUTEX_INITIALIZER** 상수를 이용해서 초기화합니다.

        이런 형식으로 사용합니다. : pthread_mutex_t lock = PTHREAD_MUTX_INITIALIZER;

        2) 동적으로 초기화하려면 pthread_mutex_init 함수를 사용하면 됩니다. mutex를 사용하기 전에 초기화를 시작해야합니다.

        ```c
        int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr);
        ```

    - pthread_mutex_lock

        pthread_mutex_lock, pthread_mutex_unlock : 이 두 함수는 mutex를 이용하여 임계 구역을 진입할때 그 코드 구역을 잠그고 다시 임계 구역이 끝날때 다시 풀어 다음 스레드가 진입할 수 있도록 합니다.

        ```c
        int pthread_mutex_lock(pthread_mutex_t *mutex);
        ```

    - pthread_mutex_unlock

        ```c
        int pthread_mutex_unlock(pthread_mutex_t *mutex);
        ```

    - pthread_mutex_destroy

        ```c
        int pthread_mutex_destroy(pthread_mutex_t *mutex);
        ```

- semaphore [https://yechoi.tistory.com/55](https://yechoi.tistory.com/55) [https://noel-embedded.tistory.com/732](https://noel-embedded.tistory.com/732)
    - sem_open

        ```c
        #include <fcntl.h>           /* For O_* constants */
        #include <sys/stat.h>        /* For mode constants */
        #include <semaphore.h>

        sem_t *sem_open(const char *name, int oflag, mode_t mode, unsigned int value);

        oflag = O_CREAT
        mode = 0644
        value = semaphore 수
        ```

    - sem_close

        ```c
        int sem_close(sem_t *sem);

        close a named semaphore
        ```

    - sem_post

        ```c
        int sem_post(sem_t *sem);

        unlock a semaphore
        ```

    - sem_wait

        ```c
        int sem_wait(sem_t *sem);

        lock a semaphore
        ```

    - sem_unlink

        ```c
        int sem_unlink(const char *name);

        remove a named semaphore

        Ctrl + C 등으로 빠져나왔을 때 name을 가진 semaphore가 그대로 남아있어서 remove 할 때 사용한다.
        ```

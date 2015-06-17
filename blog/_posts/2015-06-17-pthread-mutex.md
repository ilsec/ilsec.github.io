---

layout: post

title: pthread mutex

author: ilsec

---

##介绍

**PTHREAD_MUTEX_TIMED_NP**

>普通锁，同一线程可重复加锁，解锁一次释放锁，先等待锁的进程先获得锁。

**PTHREAD_MUTEX_RECURSIVE_NP**

>嵌套锁，同一线程可重复加锁，解锁同样次数才可释放锁，先等待锁的进程先获得锁。

**PTHREAD_ERRORCHECK_MUTEX_INITIALIZER_NP**

>纠错锁，同一线程不能重复加锁，加上的锁只能由本线程解锁，先等待锁的进程先获得锁。

**PTHREAD_ADAPTIVE_MUTEX_INITIALIZER_NP**

>自适应锁，同一线程可重加锁，解锁一次生效，所有等待锁的线程自由竞争。

##例子

下面例子演示了普通锁与嵌套锁的过程。

	#include <stdlib.h>
	#include <stdio.h>
	#include <pthread.h>
	#include <unistd.h>

	pthread_mutexattr_t attr;
	pthread_mutex_t _mutex;
	int _g = 0;
	class mutex_test{
	 public:
	  mutex_test(){}
	  ~mutex_test(){}
	
	  void test(){
	    pthread_mutex_lock(&_mutex);
	    _g++;
	  }
	};
	
	mutex_test t;
	
	void *thread_start(void *arg){
	  t.test();
	  t.test();
	}

	#define TEST_NORMAL 1
	int main(int argc, const char *argv[]){
	#ifdef TEST_NORMAL
	  _mutex = PTHREAD_MUTEX_INITIALIZER;
	#else
	  pthread_mutexattr_init(&attr);
	  pthread_mutexattr_settype(&attr, PTHREAD_MUTEX_RECURSIVE);
	  pthread_mutex_init(&_mutex, &attr);
	#endif

	  pthread_t tid;
	  pthread_create(&tid, NULL, thread_start, NULL);
	  while(1){
	    sleep(10);
	  }
	  return 0;
	}

#include <stdio.h>
#include <pthread.h>
int flag[2];
int turn,count=0,mode=0;
const int MAX = 1e9;

void lock_init()
{
    flag[0] = flag[1] = 0;
    turn = 0;
}
void lock(int curr)
{
    flag[curr] = 1;
    turn = 1-curr;
    while (flag[1-curr]==1 && turn==1-curr) ;
}

void unlock(int curr)
{
    flag[curr] = 0;
}
void* process(void *s)
{
    int i = 0;
    int curr = (int *)s;
    printf("Process : %d\n", curr);

    if(mode == 1)
        lock(curr);
    for (i=0; i<MAX; i++)
        count++;

    if(mode == 1)
        unlock(curr);
}
void main()
{
    pthread_t p1,p2,p3,p4;

    printf("\nFirst without Lock\n");
    pthread_create(&p1, NULL, process, (void*)0);
    pthread_create(&p2, NULL, process, (void*)1);
    pthread_join(p1, NULL);
    pthread_join(p2, NULL);
    printf("Actual Count: %d and Expected Count: %d\n",count, MAX*2);

    printf("\n\nNow with Lock\n");
    count = 0;
    mode = 1;
    //to switch between the 2 modes
    lock_init();

    pthread_create(&p3, NULL, process, (void*)0);
    pthread_create(&p4, NULL, process, (void*)1);
    pthread_join(p3, NULL);
    pthread_join(p4, NULL);

    printf("Actual Count: %d and Expected Count: %d\n",count, MAX*2);
}

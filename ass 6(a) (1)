#include "thread_107.h"
#include <stdio.h>
#define NUM_TICKETS 35
#define NUM_SELLERS 4

static int numTickets = NUM_TICKETS;
static Semaphore ticketsLock;

void main(int argc, char **argv)
{
int i;
char name[32];
bool verbose = (argc == 2 && (strcmp(argv[1], "-v") == 0));
InitThreadPackage(verbose);
ticketsLock = SemaphoreNew("Tickets Lock", 1);
for (i = 0; i < NUM_SELLERS; i++) {
sprintf(name, "Seller #%d", i); 
ThreadNew(name, SellTickets, 0);
}
RunAllThreads();
SemaphoreFree(ticketsLock);
printf("All done!\n");
}

static void SellTickets(void)
{
bool done = false;
int numSoldByThisThread = 0; 
while (!done) {
RandomDelay(500000, 2000000);
SemaphoreWait(ticketsLock); 
if (numTickets == 0) { 
done = true; 
} else {
numTickets--;
numSoldByThisThread++;
printf("%s sold one (%d left)\n", ThreadName(), numTickets);
}
SemaphoreSignal(ticketsLock); 
}
printf("%s noticed all tickets sold! (I sold %d myself) \n",
ThreadName(), numSoldByThisThread);
}
static void RandomDelay(int atLeastMicrosecs, int atMostMicrosecs)
{
long choice;
int range = atMostMicrosecs - atLeastMicrosecs;
PROTECT(choice = random()); 
ThreadSleep(atLeastMicrosecs + choice % range); 
}

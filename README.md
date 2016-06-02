# Roller-Coaster

The Roller Coaster Problem:

This problem consists of a roller coaster car with limited number of seats N.

The car starts and stops at the same point where the tourists are supposed to wait in a

queue till the car arrives. The car can be in either RUNNING, READY or WAITING state

defined by the CarMode variable.

The car waits until all N seats have been filled. If the car does not have enough space then

the tourists have to wait in the queue for the next trip of the car. If no tourist is available then

the car goes into sleep mode.

You have to simulate the above scenario in the Linux environment. The implementation will

consist of several processes, tables and variables. Therefore, the tables and variables

must be stored in a shared memory segment, and access protected using semaphores. A

brief description is given below.

Processes:

1. Tourist: The Tourist processes, which will be running as separate processes will be

created by the TouristArrival process. When the car starts each Tourist in the car

registers an initial emotion and stores it in a local variable INITIAL_EMOTION. It also

registers another emotion at the end of the ride and keeps it in the local variable

FINAL_EMOTION. And finally before exiting it stores them in a file with its PID as filename

(<PID>.txt). Incase a Tourist is served multiple times, multiple entries should be there in the

file. Signal handling will be required.

2. TouristArrival: This process will create the Tourist processes based on a random

probability distribution (within a time limit, s and t) and update the

Tourist­Queue­Status­Table. After creating each Tourist process it will be signalled (by

sending SIGSTOP) to remain in the blocked state, until they are woken up by the CarLoad

process. Access to the Tourist­Queue­Status­Table will be shared among the CarLoad,

CustomerArrival and DisplayQueueStatus processes. It wakes up the CarLoad

process when the CarMode variable is READY.

3. CarLoad: This process will select the Tourist processes from the

Tourist­Queue­Status­Table on FCFS basis. Their entries will be removed from the

Tourist­Queue­Status­Table and put it in the Car­Table. When all N tourists are in the

car, the selected N Tourist processes will be woken up using SIGCONT. The Tourist then

registers its INITIAL_EMOTION. Then the CarMode variable is set as RUNNING. For each

trip a fixed amount of time T is designated. Upon completion of that period, CarLoad

process will signal CarUnload process to wake up and itself go to sleep. And the

CarMode variable is set to WAITING state. If N tourists are not available in the queue then

CarLoad process will go to sleep. As soon as the required number of tourists are

available, TouristArrival process wakes up the CarLoad process. Incase total N were

already available it does not go to sleep.

4. CarUnload: It selects the Tourists who have just finished the journey and asks them

to register a FINAL_EMOTION. Next it puts their entries in the Tourist­Status­Table. Then

it selects some of the lucky tourists (with probability p) who get the chance to have a re­ride

and puts them at the back of the Tourist­Queue­Status­Table while it kills the remaining

processes. So once the ride is over, no Tourist remains in the car. The lucky ones get to

rejoin the queue and their arrival time is set. After this the CarUnload process itself goes

to sleep and sets the CarMode variable to READY state.

5. Main: This process will implement the following functionality through a continuously

running menu:

a. DisplayTouristQueueStatus: When prompted from the menu, this process will

print the status of the Tourist­Queue­Status­Table on the screen.

b. DisplayTouristStatus: When prompted from the menu, this process will print the

status of the Tourist­Status­Table on the screen.

c. DisplayCarStatus: When prompted from the menu, this process will print the

status of the Car­Table if CarMode is set as RUNNING.

Else it should display that the car is not running at that moment.

d. Exit: It should kill all the processes, namely TouristArrival, Tourists (in car as

well as Tourist­Queue­Status­Table), CarLoad and CarUnload. Then it should

properly clear the shared memory as well as semaphores and itself exit.

Data Structures:

CarMode: Set as either READY, RUNNING or WAITING.

Tourist­Queue­Status­Table:

For each newly arrived and lucky Tourist there will be one row in this table. This table will be

shared among TouristArrival, CarLoad, CarUnload and Main processes.

Each row consists of the below entries:

● PROCESS_ID

● ARRIVAL_TIME (set by TouristArrival or CarUnload)

Tourist­Status­Table:

For each served Tourist process there will be one row in this table. So Tourists who get

multiple rides will have multiple entries. This Table will be shared between CarUnload,

Tourist and Main.

Each row consists of the below entry:

● PROCESS_ID

● ARRIVAL_TIME

● START_TIME (set by CarLoad)

● END_TIME

● WAITING_TIME

Car­Table:

Size of this table will be limited to N. This table will be shared among CarLoad,

CarUnload and Main processes.

Each row consists of the below entry:

● START_TIME (set by CarLoad)

● ARRIVAL_TIME

● PROCESS_ID

Emotion File:

The file ‘emotion.txt’ contains a set of lines separated by ‘\n’

Any one of the lines is chosen at random by the Tourist processes and used as

INITIAL_EMOTION or FINAL_EMOTION.

Inputs:

N ­ Number of seats

T ­ Journey Duration

s ­ Minimum delay between two Tourist Arrivals

t ­ Maximum delay between two Tourist Arrivals

p ­ Probability of getting a re­ride.

em ­ Emotion File

Output:

The PID and Waiting Time for each served Tourist process and Average Waiting Time

for all these processes. This should be shown after you choose the Exit option.

Instructions:

Some useful system calls are:

kill, signal, shmget, shmat, shmdt, shmctl, semget, semctl, semop etc.

You should create shared memory, attach the created shared memory to address space

of calling process, detach it and remove the created shared memory segment.

Also you should create semaphore, initialize it, use it wherever required and remove it

from system. There are marks for each step.

You are free to use any data structure, but it is recommended that you use Queues and

Arrays in Shared Memory.

The Input parameters will be passed to the Main program through the command line in the

given form.

./Main <N> <T> <s> <t> <p> <em>

Do not use scanfs in the program, only command line.

Not conforming to the given standard will lead to penalties.

Your codes should adhere to one of the IPC standards like SYS V IPC.

Checking will be done on any random linux system.

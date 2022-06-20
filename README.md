# ipc_pipes_parent_child
How a parent is communicating with child via Pipes.

The code is given below

#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/wait.h>
#include <sys/ipc.h>
#define READ 0   //when zero read 
#define WRITE 1   //when one write
char *message_sent="Hi,Parent wrote to child ";

int main()
{
    int fd[2];
    char message_rcvd[100];
    pid_t pid;
    pipe(fd);
    pid=fork();
    
    if(pid>0)
    {
        printf("\nHello this is Parent writing to Child\n");
        printf("\nThe ID of Child is %d and The id of Parent is %d \n",pid,getpid());
        printf("The Message Sent to Child is :  %s\n\n", message_sent);

        close(fd[READ]);
        write(fd[WRITE],message_sent,strlen(message_sent)+1);
        close(fd[WRITE]);
    }
    else
    {
        printf("\n\nHello this is Child\n");
        close(fd[WRITE]);
        read(fd[READ],message_rcvd,100);
        printf("\n\nThe Message Recieved to Child is %s\n\n", message_rcvd);
        close(fd[READ]);
    }

    
}


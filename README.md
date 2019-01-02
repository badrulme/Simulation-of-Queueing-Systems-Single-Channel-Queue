## Simulation of Queueing Systems(Single-Channel Queue) Solved in C Program

### Example
A grocery store has one checkout counter. Customer arrive at this counter at random from 1 to 8 minutes apart and each interval time has the same probability of occurrence. The service time vary from 1 to 6 minutes, with probability give below:	


| Services (minutes)|   1  |   2  |   3  |   4  |   5  |   6  |
| ------------------|:----:|:----:|:----:|:----:|:----:|:----:|
| Probability       | 0.10 | 0.20 | 0.30 | 0.25 | 0.10 | 0.05 |


#### Simulate the arrival of 10 customers and calculate: <br>
a)	Probability that a customer has to wait	 <br>
b)	Probability of a server being idle
c)	Average service time <br>
Use the following sequence of random numbers: <br> 

| Random digit for Inter-arrival time  | 302 | 915 |  48 | 235 |  15 | 500 | 650 | 423 | 258 | 700 |
| -------------------------------------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Random digit for service time        |  83 |  45 |  74 |  65 |  17 |  79 |  30 |  61 |  89 |  20 |

Customer arrive at this counter at random from 1 to 8 minutes apart and each interval time has the same probability of occurrence.
So, Probability 1/8=0.125


| Time between arrival | Probability | Cumulative Probability  | Random digit for assignment  |
| :-------------------:|:-----------:|:-----------------------:|:----------------------------:|
|           1          |    0.125    |           0.125         |              1-125           |
|           2          |    0.125    |           0.25          |              126-250         |
|           3          |    0.125    |           0.375         |              251-375         |
|           4          |    0.125    |           0.5           |              376-500         |
|           5          |    0.125    |           0.625         |              501-625         |
|           6          |    0.125    |           0.75          |              626-750         |
|           7          |    0.125    |           0.875         |              751-875         |
|           8          |    0.125    |           1.00          |              786-000         |

   **Table: Distribution time between arrival** <br>

The service time vary from 1 to 6 minutes, with probability give below:

| Services (minutes)|   1  |   2  |   3  |   4  |   5  |   6  |
| ------------------|:----:|:----:|:----:|:----:|:----:|:----:|
| Probability       | 0.10 | 0.20 | 0.30 | 0.25 | 0.10 | 0.05 |

| Service TIme | Probability | Cumulative Probability  | Random digit for assignment |
| :-----------:|:-----------:|:-----------------------:|:---------------------------:|
|       1      |     0.10    |           0.10          |              1-10           |
|       2      |     0.20    |           0.30          |              11-30          |
|       3      |     0.30    |           0.60          |              31-60          |
|       4      |     0.25    |           0.85          |              61-85          |
|       5      |     0.10    |           0.95          |              86-95          |
|       6      |     0.05    |           1.00          |              96-00          |

**Table: Service time distribution**

## Project Flowchart
![simulation-of-queueing-systems-single-channel-queue-flowchart](https://user-images.githubusercontent.com/15130238/50600884-b728a900-0edc-11e9-98eb-3cbe327b8f8a.jpg)

## Project Code
``` c
#include<stdio.h>
#include<conio.h>
int total_waiting_time=0,total_idle_time=0,wait_count=0;
struct simulation{
    int cust_no;
    int rd_ar;
    int intr_arr_time;
    int arr_time;
    int rd_se;
    int service_begin;
    int service_time;
    int time_ser_end;
    int waiting_time;
    int idle_time;
    int spend;

};
typedef struct simulation Simulation;

void schedule_arrival_time (Simulation s[],int n);
void schedule_service_time (Simulation s[],int n);
void schedule_completion_of_service (Simulation s[],int n);
int waiting_and_idle_time (Simulation s[],int n);
int schedule_spend_time(Simulation s[],int n);

void main(){

    //freopen("result.txt", "r", stdin);
    //freopen("result.txt", "w", stdout);

    Simulation s[200];
    int i,j,n,total_service_time=0;
    float avg_waiting_time,probability_wait,probability_of_idle_server,avg_service_time,avg_time_btn_arrival,
    avg_waiting_time_those_who_wait,Average_time_customer_spends_in_the_system;
    printf(" Enter customer number: ");
    scanf("%d",&n);
    printf(" Enter random digit for arrival time for each customer: \n");
    for(i=1;i<2;i++){
      printf("  RD.Arrival[1]: -\n");
      s[i].cust_no=1;
      s[i].rd_ar=0;
      s[i].intr_arr_time=0;
    }
    for(i=2;i<=n;i++){
        printf("  RD.Arrival[%d]: ",i);
        scanf("%d",&s[i].rd_ar);
        s[i].cust_no=i;
        if(s[i].rd_ar > 0 && s[i].rd_ar < 126) s[i].intr_arr_time=1;
        else if(s[i].rd_ar > 125 && s[i].rd_ar < 251) s[i].intr_arr_time=2;
        else if(s[i].rd_ar > 250 && s[i].rd_ar < 376) s[i].intr_arr_time=3;
        else if(s[i].rd_ar > 375 && s[i].rd_ar < 501) s[i].intr_arr_time=4;
        else if(s[i].rd_ar > 500 && s[i].rd_ar < 626) s[i].intr_arr_time=5;
        else if(s[i].rd_ar > 625 && s[i].rd_ar < 751) s[i].intr_arr_time=6;
        else if(s[i].rd_ar > 750 && s[i].rd_ar < 876) s[i].intr_arr_time=7;
        else if(s[i].rd_ar > 875 && s[i].rd_ar < 1001) s[i].intr_arr_time=8;
        else{printf(" Warning!! Please Enter RD. around 1-1000"); return 0;}
    }

    schedule_arrival_time( s, n);

    printf(" Enter random digit for service time for each customer: \n");
    for(i=1;i<2;i++){
      s[i].service_begin=0;
    }
    for(i=1;i<=n;i++){
        printf("  RD.Service[%d]: ",i);
        scanf("%d",&s[i].rd_se);
        if(s[i].rd_se > 0 && s[i].rd_se < 11) s[i].service_time=1;
        else if(s[i].rd_se > 10 && s[i].rd_se < 31) s[i].service_time=2;
        else if(s[i].rd_se > 30 && s[i].rd_se < 61) s[i].service_time=3;
        else if(s[i].rd_se > 60 && s[i].rd_se < 86) s[i].service_time=4;
        else if(s[i].rd_se > 85 && s[i].rd_se < 96) s[i].service_time=5;
        else if(s[i].rd_se > 95 && s[i].rd_se < 101) s[i].service_time=6;
        else{printf(" Warning!! Please Enter RD. around 1-100"); return 0;}
        total_service_time+=s[i].service_time;

    }
    schedule_service_time( s, n);
    schedule_completion_of_service( s, n);
    schedule_service_time( s, n);
    schedule_completion_of_service( s, n);

    waiting_and_idle_time( s, n);
   int total_spend_time = schedule_spend_time(s, n);


    printf("\n   ------------------------------------------------------------------------------------------------------------------");
    printf("\n  | Cust.| RD. for | Inter_arrival | Arrival | RD. for | Service | T.Service| T.Service| Waiting | Server | Spend in |\n");
    printf("  |  No. |Arrival.T|     Time      |  Time   |Service.T|  Time   |  Begin   |    End   | in Queue| Idle.T |  System  |");
    printf("\n   ------------------------------------------------------------------------------------------------------------------\n");
    for(i=1; i<=n; i++){

        printf("     %d       %d         %d              %d          %d          %d       %d          %d            %d      %d       %d\n"
               ,s[i].cust_no,s[i].rd_ar,s[i].intr_arr_time,s[i].arr_time,s[i].rd_se,s[i].service_time,s[i].service_begin,
               s[i].time_ser_end,s[i].waiting_time,s[i].idle_time,s[i].spend);
    if(i!=n){
    printf("   ------------------------------------------------------------------------------------------------------------------\n");
        }
    else{
        printf("   --------------------------------------------------------====----------------------------====------====-----====--\n");
        }
    }

    printf("\t\t\t\t\t\t\t    %d \t\t\t\t    %d  \t       %d\t%d",
           total_service_time,total_waiting_time,total_idle_time,total_spend_time);
    avg_waiting_time = (float)total_waiting_time/(float)n;
    probability_wait = (float)wait_count/(float)n;
    probability_of_idle_server = (float)total_idle_time/(float)s[n].time_ser_end;
    avg_service_time = (float)total_service_time/(float)n;
    avg_time_btn_arrival = (float)s[n].arr_time/(float)(n-1);
    avg_waiting_time_those_who_wait = (float)total_waiting_time/(float)wait_count;
    Average_time_customer_spends_in_the_system = (float)total_spend_time/(float)n;
    printf("\n Average waiting time = %.2f",avg_waiting_time);
    printf("\n Probability(wait) = %.2f",probability_wait);
    printf("\n Probability of idle server = %.2f",probability_of_idle_server);
    printf("\n Average service time = %.2f",avg_service_time);
    printf("\n Average time between arrival= %.2f",avg_time_btn_arrival);
    printf("\n Average waiting time of those who wait= %.2f",avg_waiting_time_those_who_wait);
    printf("\n Average time customer spends in the system= %.2f",Average_time_customer_spends_in_the_system);
printf("\n\n\n\n");
    return 0;

}
void schedule_arrival_time (Simulation s[],int n){
    int i;
    for( i=2; i<=n; i++){
        s[i].arr_time=s[i-1].arr_time+s[i].intr_arr_time;
    }
}
void schedule_service_time (Simulation s[],int n){
    int i;
    for( i=2; i<=n; i++){
            if(s[i].arr_time > s[i-1].time_ser_end){
                s[i].service_begin=s[i].arr_time;
            }
            else{
                s[i].service_begin=s[i-1].time_ser_end;
            }
    }
}
void schedule_completion_of_service (Simulation s[],int n){
    int i;
    for( i=1; i<=n; i++){
        s[i].time_ser_end=s[i].service_begin+s[i].service_time;
    }
}
int waiting_and_idle_time (Simulation s[],int n ){
    int i;
    s[1].waiting_time=0;
    s[1].idle_time=0;
    for( i=2; i<=n; i++){
            if(s[i].arr_time > s[i-1].time_ser_end){
                s[i].idle_time=s[i].arr_time-s[i-1].time_ser_end;
                total_idle_time+=s[i].idle_time;
                s[i].waiting_time=0;
            }
            else{
                s[i].waiting_time=s[i-1].time_ser_end-s[i].arr_time;
                total_waiting_time+=s[i].waiting_time;
                s[i].idle_time=0;
            }
            if(s[i].waiting_time!=0){
                wait_count=wait_count+1;
            }

    }
}
int schedule_spend_time (Simulation s[],int n){
    int i,aa, total_spend_time=0;
    for(i=1;i<=n;i++)
    {
        s[i].spend = s[i].service_time+s[i].waiting_time;
        total_spend_time+=s[i].spend;

    }
    return total_spend_time;

}


```
## Program The Run
![simulation-of-queueing-systems-single-channel-queue-program-run](https://user-images.githubusercontent.com/15130238/50601206-b6444700-0edd-11e9-8e6c-ee81503c7d92.gif)


## Project Output
![simulation-of-queueing-systems-single-channel-queue](https://user-images.githubusercontent.com/15130238/50599879-8004c880-0ed9-11e9-95fb-5ac28be8dde4.png)


## Project Video
<a href="https://youtu.be/ooaJrkCxV8E">![simulation-of-queueing-systems-single-channel-queue](https://user-images.githubusercontent.com/15130238/50600510-83994f00-0edb-11e9-88d4-1318702117a6.png)</a>

##
For any query: [fb.com/badrul.me](https://www.facebook.com/badrul.me), hridoyjbd@gamil.com, Skype: hridoyjbd

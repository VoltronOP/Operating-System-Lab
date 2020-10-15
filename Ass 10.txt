// C program for implementation of FCFS 

#include<stdio.h> 
void findWaitingTime(int processes[], int n, 
						int bt[], int wt[]) 
{ 
	
	wt[0] = 0; 
	for (int i = 1; i < n ; i++ ) 
		wt[i] = bt[i-1] + wt[i-1] ; 
} 
void findTurnAroundTime( int processes[], int n, 
				int bt[], int wt[], int tat[]) 
{ 
	
	for (int i = 0; i < n ; i++) 
		tat[i] = bt[i] + wt[i]; 
} 
	
void findavgTime( int processes[], int n, int bt[]) 
{ 
	int wt[n], tat[n], total_wt = 0, total_tat = 0; 
	
	findWaitingTime(processes, n, bt, wt); 
	
	findTurnAroundTime(processes, n, bt, wt, tat); 
	
	printf("Processes Burst time Waiting time Turn around time\n"); 
	
	for (int i=0; i<n; i++) 
	{ 
		total_wt = total_wt + wt[i]; 
		total_tat = total_tat + tat[i]; 
		printf(" %d ",(i+1)); 
		printf("	 %d ", bt[i] ); 
		printf("	 %d",wt[i] ); 
		printf("	 %d\n",tat[i] ); 
	} 
	int s=(float)total_wt / (float)n; 
	int t=(float)total_tat / (float)n; 
	printf("Average waiting time = %d",s); 
	printf("\n"); 
	printf("Average turn around time = %d ",t); 
} 
	
int main() 
{ 
	
	int processes[] = { 1, 2, 3}; 
	int n = sizeof processes / sizeof processes[0]; 
	
	int burst_time[] = {10, 5, 8}; 
	
	findavgTime(processes, n, burst_time); 
	return 0; 
} 
_________________________________________________________________

// C++ program to implement Shortest Job first with Arrival Time 

#include<iostream> 
using namespace std; 
int mat[10][6]; 

void swap(int *a, int *b) 
{ 
	int temp = *a; 
	*a = *b; 
	*b = temp; 
} 

void arrangeArrival(int num, int mat[][6]) 
{ 
	for(int i=0; i<num; i++) 
	{ 
		for(int j=0; j<num-i-1; j++) 
		{ 
			if(mat[j][1] > mat[j+1][1]) 
			{ 
				for(int k=0; k<5; k++) 
				{ 
					swap(mat[j][k], mat[j+1][k]); 
				} 
			} 
		} 
	} 
} 

void completionTime(int num, int mat[][6]) 
{ 
	int temp, val; 
	mat[0][3] = mat[0][1] + mat[0][2]; 
	mat[0][5] = mat[0][3] - mat[0][1]; 
	mat[0][4] = mat[0][5] - mat[0][2]; 
	
	for(int i=1; i<num; i++) 
	{ 
		temp = mat[i-1][3]; 
		int low = mat[i][2]; 
		for(int j=i; j<num; j++) 
		{ 
			if(temp >= mat[j][1] && low >= mat[j][2]) 
			{ 
				low = mat[j][2]; 
				val = j; 
			} 
		} 
		mat[val][3] = temp + mat[val][2]; 
		mat[val][5] = mat[val][3] - mat[val][1]; 
		mat[val][4] = mat[val][5] - mat[val][2]; 
		for(int k=0; k<6; k++) 
		{ 
			swap(mat[val][k], mat[i][k]); 
		} 
	} 
} 

int main() 
{ 
	int num, temp; 
	
	cout<<"Enter number of Process: "; 
	cin>>num; 
	
	cout<<"...Enter the process ID...\n"; 
	for(int i=0; i<num; i++) 
	{ 
		cout<<"...Process "<<i+1<<"...\n"; 
		cout<<"Enter Process Id: "; 
		cin>>mat[i][0]; 
		cout<<"Enter Arrival Time: "; 
		cin>>mat[i][1]; 
		cout<<"Enter Burst Time: "; 
		cin>>mat[i][2]; 
	} 
	
	cout<<"Before Arrange...\n"; 
	cout<<"Process ID\tArrival Time\tBurst Time\n"; 
	for(int i=0; i<num; i++) 
	{ 
		cout<<mat[i][0]<<"\t\t"<<mat[i][1]<<"\t\t"<<mat[i][2]<<"\n"; 
	} 
	
	arrangeArrival(num, mat); 
	completionTime(num, mat); 
	cout<<"Final Result...\n"; 
	cout<<"Process ID\tArrival Time\tBurst Time\tWaiting Time\tTurnaround Time\n"; 
	for(int i=0; i<num; i++) 
	{ 
		cout<<mat[i][0]<<"\t\t"<<mat[i][1]<<"\t\t"<<mat[i][2]<<"\t\t"<<mat[i][4]<<"\t\t"<<mat[i][5]<<"\n"; 
	} 
} 


_________________________________________________________________

// C++ program for implementation of FCFS 
// scheduling 

#include<bits/stdc++.h> 
using namespace std; 

struct Process 
{ 
	int pid; // Process ID 
	int bt; // CPU Burst time required 
	int priority; // Priority of this process 
}; 

bool comparison(Process a, Process b) 
{ 
	return (a.priority > b.priority); 
} 

void findWaitingTime(Process proc[], int n, 
					int wt[]) 
{ 
	wt[0] = 0; 
	for (int i = 1; i < n ; i++ ) 
		wt[i] = proc[i-1].bt + wt[i-1] ; 
} 

void findTurnAroundTime( Process proc[], int n, 
						int wt[], int tat[]) 
{ 
	for (int i = 0; i < n ; i++) 
		tat[i] = proc[i].bt + wt[i]; 
} 
void findavgTime(Process proc[], int n) 
{ 
	int wt[n], tat[n], total_wt = 0, total_tat = 0; 
	findWaitingTime(proc, n, wt); 
	findTurnAroundTime(proc, n, wt, tat); 
	cout << "\nProcesses "<< " Burst time "
		<< " Waiting time " << " Turn around time\n"; 

	for (int i=0; i<n; i++) 
	{ 
		total_wt = total_wt + wt[i]; 
		total_tat = total_tat + tat[i]; 
		cout << " " << proc[i].pid << "\t\t"
			<< proc[i].bt << "\t " << wt[i] 
			<< "\t\t " << tat[i] <<endl; 
	} 

	cout << "\nAverage waiting time = "
		<< (float)total_wt / (float)n; 
	cout << "\nAverage turn around time = "
		<< (float)total_tat / (float)n; 
} 

void priorityScheduling(Process proc[], int n) 
{ 
	sort(proc, proc + n, comparison); 

	cout<< "Order in which processes gets executed \n"; 
	for (int i = 0 ; i < n; i++) 
		cout << proc[i].pid <<" " ; 

	findavgTime(proc, n); 
} 

int main() 
{ 
	Process proc[] = {{1, 10, 2}, {2, 5, 0}, {3, 8, 1}}; 
	int n = sizeof proc / sizeof proc[0]; 
	priorityScheduling(proc, n); 
	return 0; 
} 

_________________________________________________________________

// C++ program for implementation of RR scheduling 

#include<iostream> 
using namespace std; 
void findWaitingTime(int processes[], int n, 
			int bt[], int wt[], int quantum) 
{  
	int rem_bt[n]; 
	for (int i = 0 ; i < n ; i++) 
		rem_bt[i] = bt[i]; 

	int t = 0; 
	while (1) 
	{ 
		bool done = true; 
		for (int i = 0 ; i < n; i++) 
		{ 
			if (rem_bt[i] > 0) 
			{ 
				done = false; // There is a pending process 

				if (rem_bt[i] > quantum) 
				{ 
					t += quantum; 
					rem_bt[i] -= quantum; 
				} 
				else
				{ 
					t = t + rem_bt[i]; 
					wt[i] = t - bt[i]; 
					rem_bt[i] = 0; 
				} 
			} 
		} 
		if (done == true) 
		break; 
	} 
} 
void findTurnAroundTime(int processes[], int n, 
						int bt[], int wt[], int tat[]) 
{ 
	for (int i = 0; i < n ; i++) 
		tat[i] = bt[i] + wt[i]; 
} 
void findavgTime(int processes[], int n, int bt[], 
									int quantum) 
{ 
	int wt[n], tat[n], total_wt = 0, total_tat = 0; 
	findWaitingTime(processes, n, bt, wt, quantum); 
	findTurnAroundTime(processes, n, bt, wt, tat); 
	cout << "Processes "<< " Burst time "
		<< " Waiting time " << " Turn around time\n"; 
	for (int i=0; i<n; i++) 
	{ 
		total_wt = total_wt + wt[i]; 
		total_tat = total_tat + tat[i]; 
		cout << " " << i+1 << "\t\t" << bt[i] <<"\t "
			<< wt[i] <<"\t\t " << tat[i] <<endl; 
	} 

	cout << "Average waiting time = "
		<< (float)total_wt / (float)n; 
	cout << "\nAverage turn around time = "
		<< (float)total_tat / (float)n; 
} 
int main() 
{ 
	int processes[] = { 1, 2, 3}; 
	int n = sizeof processes / sizeof processes[0]; 
	int burst_time[] = {10, 5, 8}; 
	int quantum = 2; 
	findavgTime(processes, n, burst_time, quantum); 
	return 0; 
} 


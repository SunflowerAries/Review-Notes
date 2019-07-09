# <center>Notes for locks</center>

[TOC]

## Peterson

```
void enter(int process)
{
	int other;
	other = 1 - process;
	ready[process] = true;
	turn = other;
	while(turn == other && ready[other] == true)
		;
}
```

## Deadlock

### Necessary and sufficient conditions

- Mutual exclusion
- Hold and wait
- No preemption
- Circular wait

### Solutions

#### Deadlock prevention

- For hold and wait
  - Only without resources can apply for the resources
  - Apply for all the resources that need
  - Otherwise, suspend
- For no preemption
  - If not successfully apply for the resources, then give up all the resources
  - Predation, the system help to fetch resources from the inactive processes, otherwise restart.

#### Deadlock avoidance



#### Deadlock detection and recovery

#### The ostrich strategy


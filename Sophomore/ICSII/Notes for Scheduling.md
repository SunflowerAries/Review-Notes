# <center>Notes for Scheduling</center>

[TOC]

## KPI

- Turnaround time: from submit to finish

## First-come-first-served

## Shortest-job-first

To predict the time that next job will take

$\tau_{n + 1} = \alpha t_n + (1 - \alpha) \tau_n$

## Round-robin

time-sharing

## Priority-based

## Highest response ratio next

$R = \frac{WaitingTime + ExpectedServiceTime}{ExpectedServiceTime}$

## Multilevel-queue

- preemptive
- FCFS in the respective queue

## Multilevel-feedback-queue

- High Priority first
- Low priority has longer time slice
- When first come, be entitled with the highest
- Equal priority, round-robin
- If a process runs out of the slice, it will be degraded
- After a time, the lowest will back to the highest

## Real-time

### Rate-Monotonic Scheduling

The longer period $p$ the higher the priority $\frac{1}{p}$

 ### DDL
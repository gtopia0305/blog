---
description: 슈퍼컴퓨팅인프라센터 2021. 11. 12. 14:00
---

# 뉴론 SLURM 스케줄러 Fairshare 스케줄링 정책

## **1. SLURM 스케줄러의 Fairshare 스케줄링 정책**

Fairshare는 모든 사람이 클러스터 시스템을 사용할 수 있는 공정한 기회를 얻게 해주는 스케줄링 정책입니다. 사용자의 시스템 사용량을 기반하여 Fairshare 점수를 계산해내고, 이 점수를 기반으로 대기 작업에 대한 할당의 우선순위가 정해집니다. 구체적으로, 시스템을 조금 더 많이 사용한 사용자에 비해 시스템을 사용하지 않은 사용자의 대기 작업이 더 높은 우선순위를 얻게 해줍니다.



## **2. 사용자별 Fairshare 값 확인 방법**

```
$ slurm_fs_check

       User        RawUsage       FairShare
 -----------------------------------------------
    testuser         2696014        0.835264
```

위 테이블에서 Fairshare 값이 높은 사용자일수록 대기 작업이 더 높은 우선순위를 얻게 됩니다.



## **3. Fairshare 값을 계산하기 위한 시스템 사용량 감소 주기**

일주일 간격으로 시스템 사용량(위 표에서 RawUsage 값)이 1/2로 감소합니다. 시스템 사용량이 감소하게 되면, Fairshare 값이 높아지면서 대기 작업에 대한 우선순위도 점차적으로 높아지게 됩니다.

***

****

**Fairshare Scheduling Policy in NEURON SLURM Scheduler**

#### **1. Fairshare Scheduling Policy in SLURM Scheduler**

Fairshare is a scheduling policy that gives everyone a fair opportunity to use a cluster system. It calculates a Fairshare score based on the user's system usage, and based on this score, priority of allocations to pending jobs. Specifically, it allows waiting tasks for users who are not using the system to be given a higher priority compared to users who have been using the system a little more.



**2. How to check Fairshare value by user**

```
$ slurm_fs_check
          User        RawUsage       FairShare
-----------------------------------------------
       testuser         2696014        0.835264
```

In the table above, users with higher Fairshare values ​​will get queued jobs higher priority.



**3. System usage reduction cycle to calculate Fairshare values.**

The system usage (RawUsage values in the table above) is reduced to 1/2 every week. As the system usage decreases, the Fairshare value increases, which gradually increases the priority of waiting jobs.

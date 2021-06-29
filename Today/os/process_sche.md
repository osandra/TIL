### CPU 스케줄링
---
CPU 스케줄러는 메모리에 로드된 프로세스에서 실행할 프로세스를 고르는 스케줄링 작업을 진행한다. 

CPU 스케줄링의 목적에는 CPU 활용 최대화, 공정한 CPU 할당, 대기 시간의 최소화, 응답 시간 최소화(첫 응답 생성 시간), 안정성 확보(우선순위 사용해서 중요프로세스가 먼저 작동하도록 배정), 무한 연기 방지, throughput 최대화 등이 있다.
- throughput: 시간 당 실행 완료하는 프로세스 수


스케줄링 방식은 크게 **선점형 스케줄링, 비선전형 스케줄링으로 나눌 수 있다.** 

- **`선점형`**: 시분할 시스템 고려하여 만들어진 알고리즘으로 어떤 프로세스가 CPU를 할당받아 실행중이라도 운영체제가 `CPU 를 강제로 빼앗을 수 있다.(인터럽트 처리)`

    문맥 교환 같은 부가적인 작업으로 인해 낭비(**오버헤드**)가 생기는 것이 단점이다. **하나의 프로세스가 CPU를 독점할 수 없기에 빠른 응답시간을 요구하는 대화형 시스템이나 시분할 시스템에 적합하다.**
- **`비선점형`**: 프로세스가 CPU를 할당받으면 작업이 끝날 때까지 **`CPU를 놓지 않는 것(계속 점유)`을 말한다**. 즉 어떤 프로세스가 CPU를 점유하면 다른 프로세스가 이를 빼앗을 수 없음 → **문맥교환에 의한 낭비는 적지만 사용시간 긴 프로세스 때문에 CPU 사용시간 짧은 여러 프로세스가 기다리게 되어 전체 시스템 처리율이 낮다.  ex)** 10초 소요되는 프로세스가 실행되고 있고, ready queue에 1초 소요되는 프로세스들이 기다리고 있는 상황

---
### 스케줄링 알고리즘 (FCFS, SJF, RR,SRT,우선순위, 다단계 큐, 다단계 피드백 큐)

우선순위 스케줄링은 둘 다 가능하다.
| 방식 | 알고리즘 이름 |
|---|:---:|
| `비선점형 알고리즘` | FCFS 스케줄링, SJF 스케줄링 | 
| `선점형 알고리즘` | 라운드 로빈 스케줄링, SRT 스케줄링, 다단계 피드백 스케줄링 | 
우선순위 스케줄링은 둘 다 가능하다.

*****
<br>

- `FCFS 스케줄링:` 단일 큐에서 프로세스가 큐에 도착한 순서대로 실행되는 방식. 비선점형이므로 해당 프로세스가 끝나야 다음 프로세스를 실행할 수 있다. 따라서 긴 프로세스가 먼저 들어올 경우, 다른 프로세스가 오래 기다려야 하므로 시스템 효율성이 떨어지는 문제가 있다. 이처럼 느린 프로세스로 인해 전체 운영체제 속도가 느려지는 효과를 `Convoy effect(호송 효과)`라고 한다.
- `SJF(Short Job First)` 스케줄링: 프로세스 중 실행시간이 가장 짧은 작업부터 CPU를 할당하는 비선점형 방식을 말한다.  짧은 프로세스 부터 실행하기에 효율성이 좋지만 레디큐에 있는 프로세스의 실행시간을 정확하게 예측하기 어렵다는 단점이 있다. 또한 실행시간이 오래 걸리는 작업이 계속 연기되는 아사 현상(starvation)이 발생한다는 문제도 있다.

    아사 현상은 프로세스가 양보할 수 있는 상한선을 정하는 방식(aging)을 사용해 완화할 수 있지만 잘 사용하지 않는다.

- `라운드 로빈 스케줄링(RR)`: FCFS 스케줄링과 유사하지만 큰 차이점은 **타임 슬라이스를 이용해 할당 시간 동안 작업을 완료하지 못하면  레디 큐로 돌아가는 것이다.** 즉 선점형 방식으로, 주어진 타임 슬라이스 동안만 작업할 수 있다. (우선순위 적용 X) 타임 슬라이스의 크기는 문맥 교환에 걸리는 시간을 고려하여 적당하게 설정하는 것이 중요하다.
- `SRT 스케줄링(Shortest Remaining Time):` 이 방식에선 완료될 때까지 남은 시간이 가장 짧은 프로세스를 실행한다는 점에서 SJT와 비슷하지만 선점형 방식을 따른다는 차이가 있다.
SRT에서는 새로 들어온 프로세스의 예상 남은 시간(완료되기까지 시간)이 현재 실행중인 프로세스의 예상 시간보다 적은 경우, 이들끼리 문맥교환을 진행한다. 따라서 SRT 방식은 긴 프로세스가 짧은 프로세스들에 의해 무기한 연기될 수 있다는(starvation) 단점이 있다.
- `우선순위 스케줄링`: 우선순위를 반영한 스케줄링 알고리즘을 말한다. 만약 프로세스의 우선순위가 동일하다면 FCFS와 같은 순서로 진행된다. 해당 방식에서도 우선순위가 낮은 프로세스가 연기될 수 있다는 단점이 있다. (aging 으로 완하 가능)

- `다단계 큐 스케줄링`: 우선순위에 따라 준비 큐를 여러 개 사용하는 방식을 말한다. 프로세스는 부여받은 우선순위에 따라 해당 우선순위 큐에 삽입된다. 상단의 큐에 있는 모든 프로세스의 작업이 끝나야 다른 우선순위 큐가 시작된다. 각각의 큐는 이에 맞는 다른 스케줄링 방식을 적용할 수 있기에 전체 CPU의 효율성을 높일 수 있다. 

- `다단계 피드백 큐 스케줄링`: 다단계 큐 스케줄링과 기본적인 형태가 같지만 CPU를 사용하고 난 프로세스가 `원래의 큐로 되돌아가지 않고 우선순위가 하나 낮은 큐의 끝으로 들어가는 방식`이다.
각 큐는 라운드 로빈 방식을 사용하며 우선순위가 낮은 큐일 수록 타임 슬라이스가 커지는 편이다. 따라서 프로세스에서 CPU 시간을 너무 많이 사용하면 우선 순위가 낮은 대기열로 계속 이동할 수 있으므로 유연성이 높다. 그러나 프로세스가 서로 다른 대기열에서 이동하면서 CPU 오버헤드가 더 많이 발생한다는 단점이 있다.

<details>
<summary>출처</summary>

- [https://www.tutorialspoint.com/operating_system/os_process_scheduling_qa3.htm](https://www.tutorialspoint.com/operating_system/os_process_scheduling_qa3.htm)

- [https://www.faceprep.in/operating-systems/operating-systems-convoy-effect/](https://www.faceprep.in/operating-systems/operating-systems-convoy-effect/)

- [https://www.geeksforgeeks.org/cpu-scheduling-in-operating-systems/?ref=rp](https://www.geeksforgeeks.org/cpu-scheduling-in-operating-systems/?ref=rp)

- [https://www.studytonight.com/operating-system/multilevel-queue-scheduling](https://www.studytonight.com/operating-system/multilevel-queue-scheduling)

- [https://www.studytonight.com/operating-system/multilevel-feedback-queue-scheduling](https://www.studytonight.com/operating-system/multilevel-feedback-queue-scheduling)
</details>
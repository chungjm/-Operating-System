## 컴퓨터 시스템의 동작

### Instruction and data

Instruction and data

- 입력 장치를 통해 컴퓨터에 유입되는 정보

- Instruction은 processor가 실행할 산술∙논리 연산의 동작을 명시하는 명령어이며, 어떤 작업을 수행하는 instruction 집합이 program

- High-level program은 컴파일러 등을 이용하여 0과 1로 구성된 binary instruction으로 변환해야 컴퓨터가 실행 가능

```
컴퓨터 시스템의 작업 처리 순서
1. 명령어를 메모리에서 읽어옴
2. 레지스터/메모리에 저장한 정보를 명령어 제어 따라 읽어 연산장치에서 처리
3. 처리한 정보를 레지스터/메모리에 저장

입력 장치로 받은 정보는 메모리에 저장
메모리의 데이터는 필요시 출력장치에 표시하거나 저장장치에 저장
```

---

### 명령어의 구조

OPcode + 주소부(목적지 피연산자, 소스 피연산자)

- OPCode(Operation code)
  프로세서가 실행할 동작인 연산 지정
- 산술연산(+, -, *, /) 논리연산(and, or, not), shif 보수등 연산 정의
- 연산 부호가 n 비트이면 최대 2^n개 연산이 가능

- Operand
  - 연산할 데이터 정보 저장
  - 데이터는 레지스터나 메모리, virtual memory, 입출력 장치등에 위치할 수 있는데 보통 데이터 자체보다는 데이터의 주소를 저장

```
operand의 위치를 명시하는 방법: 직접 주소, 간접 주소
이는 mod bit 을 추가하거나, 다음 명령어의 위치를 나타내는 주소를 추가한다

왜 indirect를 쓰는가 ?
    operand에 들어갈 수 있는 크기가 전체 bit를 다 사용할 수가 없고 operand가 여러 개가 들어올 때도 있기에
    또한 직접 주소 방식에는 메모리 공간이 한정적이므로 이를 벗어나면 간접 주소 방식을 통해 이용한다.
```

### 명령어의 실행

1. Instruction fetch `명령어 레지스터에 저장된 다음 명령어를 인출한다`
2. 명령어 해석, 프로그램 카운터 변경 `인출한 명령어를 해석하고 다음 명령어를 지정하려고 프로그램 카운터를 변경한다`
3. Operand fetch `명령어가 메모리에 있는 워드를 한개 사용하려면 사용 장소를 결정하여 피연산자를 인출하고, 필요하면 프로세서 레지스터로 보낸다.`
4. 명령어 실행
5. 결과 저장
6. 다음 명령어로 이동 `위의 과정을 반복`

---

### 주변장치와의 통신

Polling
- CPU가 입출력장치의 상태를 주기적으로 검사하여 일정한 조건을 만족할 때 데이터를 처리

- CPU가 명령어 해석과 실행이라는 본래 역할 외에 모든 입출력까지 관여해야 하므로 작업 효율이 떨어짐

Interrupt(Polling에서 진화)
- Peripheral이 CPU의 개입이 필요하면 직접 알려주는 방식
- CPU는 Interrupt 요청(IRQ)이 있으면 peripheral에 접근하여 필요한 작업을 처리
- CPU는 IRQ를 기다리는 동안 다른 작업을 할 수 있음
- IRQ가 Peripheral에서 돌아오면 IRQ 작업을 다시 프로세서가 실행하는 context switching이 필요함

```
Peripheral은 스스로 데이터를 processor나 memory에 전달하는 기능이 없음
- processor가 시킨 작업이나 외부 입력에 의한 결과 처리는 processor가 해야함

예상치 못한 사용자 입력, 입출력 작업 완료와 같은 상황을 운영체제가 적절히 처리하는 과정이 필요
- interrupt를 통해 processor에게 상태를 알림
- IRQ 신호선을 통해 이루어짐(시스템 버스와 다른 신호선)
- 예) 키보드에서 입력이 발생했을 때만 프로세서에 통보하여 처리하므로, 프로세서가 이벤트 발생 여부를 일일이 감시하지 않아도 됨

IRQ 신호에 따라 인터럽트 처리 프로그램(IRQ handler, interrupt service routine) 수행
- 현재 실행 중인 프로세스를 중단하고 IRQ handler의 실행을 요구

Interrupt and exception
공통점: 현재의 프로세스 수행을 중지하고 정해진 handler를 실행
interrupt: 대부분의 장치는 이벤트 발생 시 프로세서에 interrupt 신호를 보냄
exception: 오류에 대한 반응으로 발생. (Memory/instruction error, operation error(dov/0), system call
```

### Memory Mapped I/O

메모리의 일정 공간을 입출력에 할당하는 기법
- CPU 입장에서 봤을 때는 peripheral 도 메모리로 보이는 것
- 메모리 접근하는 것과 동일한 방식으로 주소만 peripheral에 해당하는 주소로 접근하면 주변 장치를 제어할 수 있기 때문이다.
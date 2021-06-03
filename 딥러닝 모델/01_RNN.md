# RNN

### RNN은?

### RNN (Recurrent Neural Network) - 순환신경망

- sequence 모델 : 입/출력을 시퀀스 단위로 처리 → 맥락/순차가 있는 series 데이터를 사용
- 은닉층에서 활성화 함수를 지닌 값이 출력층 방향으로 가는 Feed Forward Neural Network와 달리 은닉층에서 활성화 함수를 지닌 값이 출력층으로도 가고, 다음 은닉층의 입력으로도 가는 특징을 가진 RNN → 순환

    ![RNN%2088722b299d154c008b3c5423b22724a3/Untitled.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled.png)

    은닉층에서 활성화함수를 통해 결과를 출력하는 역할 : cell (=RNN cell, 메모리 cell)→ 이전 값을 기억하는 메모리 역할 

    ⇒ cell은 각 시점 (time step)에서 이전 시점의 메모리 셀에서 나온 값인 은닉 상태 (hidden state)를 자신의 입력으로 사용하는 재귀적 활동을 함

![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%201.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%201.png)

- RNN은 입출력 길이를 다르게 설계 할 수 있음
    1. one-to-many : image captioning에 사용 (한 이미지 입력에 대해 사진의 제목 출력)
    2. many-to-one : 감성 분류, 스팸 메일 분류에 사용
    3. many-to-many : 챗봇, 번역기 등에 사용

        ![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%202.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%202.png)

### RNN 수식

현재 시점 t에서의 은닉 상태값을 ht라 할 때, 메모리 셀이 ht를 계산하기 위해 총 두 개 가중치가 필요 - Wx, Wh

Wx : 입력층에서 입력값을 위한 가중치

Wh : 이전 시점 t-1의 은닉 상태값인 ht-1을 위한 가중치

![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%203.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%203.png)

활성화 함수 F : 하이퍼볼릭탄젠트(비선형 함수)

Wh : 출력층에서 출력값을 위한 가중치

![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%204.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%204.png)

단어 벡터의 차원이 d, 은닉 상태의 크기가 Dh라 했을 때, 각 벡터의 행렬 크기 :

![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%205.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%205.png)

### RNN의 한계

단순한 형태의 RNN을 바닐라 RNN이라고 함

바닐라 RNN의 단점 : 장기 의존성 문제 = long-term dependency

⇒ time step이 길어질 수록 앞의 정보가 충분히 전달되지 못함 

⇒ 즉 문장이 길 수록 단어를 예측하기 어려움

### Deep Recurrent Neural Network (깊은 순환 신경망)

- 다수의 은닉층을 가짐

![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%206.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%206.png)

은닉층이 2개

### Bidirectional Recurrent Neural Network (양방향 순환 신경망)

- 이전 시점의 메모리 셀뿐만 아니라 이후의 메모리 셀로도 예측할 수 있음
- 아래 그림의  주황색 메모리 셀이 앞 시점의 은닉 상태, 초록색 메모리 셀이 뒤 시점의 은닉 상태를 전달 받음

![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%207.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%207.png)

### 파이썬으로 RNN 구현

![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%208.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%208.png)

 출력

![RNN%2088722b299d154c008b3c5423b22724a3/Untitled%209.png](RNN%2088722b299d154c008b3c5423b22724a3/Untitled%209.png)
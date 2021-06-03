# LSTM / GRU

### LSTM (Long Short-Term Memory) - 장단기 메모리

RNN의 한계 : time step이 길어질 수록 앞의 정보가 충분히 전달되지 못함 (장기 의존성 문제 = long-term dependencies)

- 은닉층의 메모리 셀에 3개 게이트 input, forget, output gate를 추가해서 불필요한 것은 지우고 기억할 것은 정함 → cell state를 추가함 (t시점의 셀 상태를 Ct)

![LSTM%20GRU%20c29f7aab49804f51a4e41706d5c8e763/Untitled.png](LSTM%20GRU%20c29f7aab49804f51a4e41706d5c8e763/Untitled.png)

입력 게이트 - 현재 정보를 기억하는 게이트

삭제 게이트 - 기억을 삭제하기 위한 게이트

출력 게이트 - 현재 시점 t의 x값과 이전 시점 t-1의 은닉 상태가 시그모이드 함수를 지난 값

### GRU (Gated Recurrent Unit)

- LSTM의 성능과 유사하면서 LSTM의 구조를 간단화
- 2개 gate, 업데이트 게이트와 리셋 게이트
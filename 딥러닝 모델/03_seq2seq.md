# seq2seq

### Sequence-to-Sequence; seq2seq

- 번역기에서 대표적으로 사용되는 모델
- seq2seq의 구조

    encoder와 decoder로 구성됨

    ![seq2seq%20e73642a2bebc450cbea5d549e86f5d28/Untitled.png](seq2seq%20e73642a2bebc450cbea5d549e86f5d28/Untitled.png)

    입력 문장의 모든 단어를 순차적으로 입력 받고, 마지막에 모든 단어 정보들을 압축해서 하나의 vector로 만듦 → context vector

    ![seq2seq%20e73642a2bebc450cbea5d549e86f5d28/Untitled%201.png](seq2seq%20e73642a2bebc450cbea5d549e86f5d28/Untitled%201.png)

    encoder의 마지막 셀의 hidden state를 decoder 셀로 전달 : context vector

    1. decoder의 초기 입력은 문장을 시작하는 심볼 <sos> 

        → 다음에 등장할 확률이 높은 단어를 예측 (= je)

    2. 예측된 단어 je를 다음 시점의 RNN셀의 입력으로 사용
    3. 문장의 끝을 의미하는 심볼 <eos>가 예측될 때까지 반복

    RNN셀은 현재 시점을 t라고 할 때, t-1의 hidden state와 t의 입력 vector를 입력으로 받음

    → 과거 시점의 hidden state들의 값이 누적되기 때문에 encoder의 마지막 셀의 hidden state는 모든 단어들의 정보를 담고 있음

    decoder의 단어 예측 부분

    출력 vector 중에서 softmax함수를 통해 각 단어별 확률값을 반환하고, 출력 단어 결정

    ![seq2seq%20e73642a2bebc450cbea5d549e86f5d28/Untitled%202.png](seq2seq%20e73642a2bebc450cbea5d549e86f5d28/Untitled%202.png)

[위키독스](https://wikidocs.net/24996)
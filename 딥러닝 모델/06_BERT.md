# BERT

### BERT (Bidrectional Encoder Representations from Transformers)

대용량 unlabled data로 모델을 학습시킨후, 특정 task를 가지고 있는 labeled data로 transfer learning하는 모델

BERT이전에는 language 모델을 학습하고, 뒤쪽에 특정 task를 처리하는 network를 붙이는 방식 (ELMo, GPT ..)

BERT는 새로운 network를 붙일 필요 없이 BERT 모델 자체의 fine-tuning을 통해 task 처리

![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled.png)

BERT의 프로세스

- model architecture

    transformer의 encoder 부분만 사용

    ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%201.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%201.png)

    모델의 크기에 따라 분류

    - BERT_base : layer = 12, hidden size = 768, self-attention head 수 = 12, 전체 파라미터 = 1.1억 (GPT와 동일한 사이즈)
    - BERT_large : layer = 24, hidden size = 1024, self-attention head 수 = 16, 전체 파라미터 = 3.4억

- Input embedding

    ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%202.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%202.png)

    3가지 입력 임베딩을 취합해 하나의 임베딩 값을 만듦 + layer normalization + dropout 적용 ⇒ 입력

    1. token embedding

        WordPiece임베딩 방법을 사용 : 자주 등장하면서 가장 긴 길이의 sub-word를 하나의 단위로 만들고, 자주 등장하지 않는 rare-word를 sub-word로 쪼개서 만든다. 

        두 가지 특수 토큰 (CLS, SEP)을 사용해 문장을 구별

        기존 임베딩 방법과 다르게 Out-of-Vocabulary(OOV) 문제가 없어짐

    2. segment embedding

        두 개의 문장을 문장 구분자([SEP])와 합쳐서 입력

        입력 길이는 두 문장을 합쳐서 512 subword 이하로 제한

    3. position embedding

        self-attention모델을 사용하기 위해 입력 토큰의 위치 정보를 줘야함

        transformer에서는 sinusoid함수를 이용해 positional encoding을 하고, BERT에서는 이를 변형해서 position encoding 사용 (단순히 token 순서대로 0, 1, 2 ..)

- pre-training BERT : unsupervise 방법 2가지
    1. 기존 방법

        ELMo나 GPT는 language model을 사용 : 앞의 n개 단어를 가지고 뒤의 단어 예측 → unidirectional (단방향) : left to right 이나 right to left

    2. Masked Language Model (MLM) 방법

        input에서 무작위로 몇개의 token을 mask시키고 transformer의 encoder 구조에 입력하면 주변 단어의 문맥만을 보고 mask된 단어 예측 → 문맥을 파악하는 능력을 기름

        ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%203.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%203.png)

        ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%204.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%204.png)

        단어의 15%를 mask token으로 변경해서 mask token만을 예측하는 pre-training task 수행

        → 이때, 80% : mask token, 10% : token을 random 단어로 변경, 10% : token을 원래 단어 그대로

        mask token은 fine-tuning시에는 사용되지 않음

    3. Next Sentence Prediction (NSP) 방법

        두 문장을 pre-training 시 같이 넣고, 두 문장이 이어지는 문장인지 아닌지 맞추는 것 → QA나 Natural Language Inference(NLI)와 같이 두 문장 사이 관계가 중요한 모델에 필요

        ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%205.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%205.png)

        50%는 두 문장이 실제로 연관이 있는 예제, 50%는 두 문장이 서로 연관이 없는 예제

- pre-training 절차
    1. NSP를 위해 문장을 뽑아서 임베딩
    2. Masking 작업
    3. 하이퍼파라미터 : 배치 사이즈 256, Adam 옵티마이저, dropout 0.1, gelu 활성화 함수 사용

- fine-tuning (transfer learning : 전이학습)

    ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%206.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%206.png)

    11개 task에 대한 fine-tuning

    a) 문제 쌍 분류 문제로 두 문장을 하나의 입력으로 넣고 두 문장의 관계를 구함

    b) 한 문장을 입력으로 넣고 문장의 종류 분류

    c) 문장이나 문단 내에서 원하는 정답 위치의 시작과 끝을 구함

    d) 입력 문장의 token명이나 품사를 구하는 문제

    ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%207.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%207.png)

- Ablation 연구
    - effect of pre-training task

        ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%208.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%208.png)

        BERT의 두가지 학습방법 (MLM, NSP)를 제거하며 결과 비교

    - effect of model size

        ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%209.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%209.png)

        모델이 커질수록 정확도 상승

    - effect of number of training steps

        ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%2010.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%2010.png)

    - feature-based approach with BERT

        ELMo와 같이 특정 NLP task를 수행할 수 있는 network를 부착하여 쓸 수 있음

        ![BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%2011.png](BERT%20f39c7ff02858442291880ad8a35edd1a/Untitled%2011.png)

        finetune all과 성능 차이가 별로 없다

    [BERT 논문정리](https://mino-park7.github.io/nlp/2018/12/12/bert-%EB%85%BC%EB%AC%B8%EC%A0%95%EB%A6%AC/?fbclid=IwAR3S-8iLWEVG6FGUVxoYdwQyA-zG0GpOUzVEsFBd0ARFg4eFXqCyGLznu7w#35-fine-tuning-procedure)
# 개요

오늘은 NLP 분야 유명한 논문인 Sequence to Sequence Learning with Neural Networks에 대하여 논문과 이에 관련한 강의를 들었다.
Sequence to Sequence 논문에서는 신경망을 이용하여 입력 시퀀스를 출력 시퀀스로 매핑하는 일반적인 방법을 제안한다. 이 모델은 인코더-디코더 구조를 사용하여 기계 번역, 질의응답 등 다양한 sequence-to-sequence 문제를 해결할 수 있는 범용적인 프레임워크를 제시했다. 특히 LSTM을 활용하여 긴 시퀀스에서도 효과적으로 학습할 수 있음을 보여주었으며 이는 현대 자연어 처리의 기반이 되는 중요한 연구로 평가받고 있다.

# 논문을 보기 전

본격적인 논문 내용을 살펴보기 전에 논문에서 자주 언급되는 Sequence data와 LSTM 개념을 간단히 정리하겠다.

## Sequence data
Sequence data란 순서대로 정렬된 데이터의 연속이다. 예를 들어, 순서가 있는 문자열, 시간에 따라 달라지는 주식 가격과 같은 시계열 데이터 등이 있다.
Sequence data는 RNN과 적합하며 DNN과는 적합하지 않다. 이는 RNN이 순차적인 데이터에서 이전 정보들을 활용하여 학습할 수 있는 구조를 가지고 있기 때문이며 DNN은 독립적인 입력 데이터에 더 적합하기 때문이다.

## LSTM
LSTM 이란 Long-Short-Term-Memory의 약자로 일반 RNN의 문제들을 보완한 RNN 계열 Neural-net이다. LSTM은 우리 사람의 뇌와 같이 중요한 정보는 오래 기억하고 불필요한 정보는 잊어버리도록 설계되어있다.
LSTM은 이전 hidden state와 input에서 들어온 새로운 정보는 각각 forget gate, input gate, output gate를 통해 처리되며, 이들의 결과는 cell state를 업데이트하여 장기 기억을 유지하거나 수정한다.
작동과정을 살펴보면,
1. Input gate
    - 현재 입력(Input)과 이전 hidden state에서 받은 정보가 결합되어 시그모이드 활성화 함수를 통과한다. 이 결과는 0~1 사이의 값으로 새로운 정보를 얼마나 cell state에 추가할 지를 결정한다.
2. Forget gate
    - 이전 hidden state에서 받은 정보와 현재 입력 받은 정보가 시그모이드 활성화 함수를 통과하여 0~1의 값을 생성하고 이 값은 이전 cell state에서 어떤 정보를 잊을지 결정한다.
3. Cell state 업데이트
    - forget gate의 출력과 이전 cell state가 곱해져 잊어야 할 정보를 제거한다. input gate의 출력과 새로운 정보가 곱해져 cell state에 추가된다.
4. Output Gate
    - 현재 입력과 이전 hidden state가 결합되어 시그모이드 활성화 함수를 통과한다. 이 값은 최종적으로 출력될 정보를 결정한다. 업데이트된 cell state는 tanh 활성화 함수를 통과하여 output gate의 결과와 곱해져 최종 hidden state를 생성한다.

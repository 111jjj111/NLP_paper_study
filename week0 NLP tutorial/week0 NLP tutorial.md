# NLP 핵심 개념 정리

## 1. Tokenization (토큰화)

### 정의
텍스트를 **의미 있는 단위(토큰)** 로 나누는 작업

### 필요한 이유
- 컴퓨터는 자연어(문장)를 의미 없는 문자열로 보기 때문에 의미 단위로 나누어 분석하기 위함

### 종류
- **띄어쓰기 기준**: Whitespace Tokenization
- **단어 단위**: Word Tokenization (NLTK, spaCy 등)
- **형태소 단위** (한국어): Mecab, Khaiii, Okt 등
- **문자 단위**: Character Tokenization
- **Subword 단위**: BPE, WordPiece (BERT 등에서 사용)

### 코드 예시
```python
from nltk.tokenize import word_tokenize
text = "I love NLP!"
tokens = word_tokenize(text)
# 결과: ['I', 'love', 'NLP', '!']
```

---

## 2. Vectorization (벡터화)

### 정의
토큰들을 **수치 벡터**로 변환하는 작업

### 벡터화 방법

#### ① 통계와 머신러닝 기반
- Bag-of-Words (BoW)
- TF-IDF
- LSA (Latent Semantic Analysis)
- LDA (Latent Dirichlet Allocation)

#### ② 인공 신경망 기반 (Neural Embeddings)
- Word2Vec (CBOW, Skip-gram)
- GloVe
- FastText
- Transformer 기반 (BERT, GPT 등)

---

## 3. Bag of Words (BoW)

텍스트에서 단어의 **빈도**만 세고, 순서는 무시하는 방식

예시:
```python
"John likes to watch movies. Mary likes movies too."
# {'John':1, 'likes':2, 'to':1, 'watch':1, 'movies':2, 'Mary':1, 'too':1}
```

---

## 4. Document-Term Matrix (DTM)

문서와 단어 간의 관계를 행렬로 표현한 것

| 문서 \ 단어 | 나는 | 너는 | 밥을 | 먹었다 | 안 | 운동을 | 했다 |
|-------------|------|------|------|--------|----|-------|------|
| 문서1       | 1    | 0    | 1    | 1      | 0  | 0     | 0    |
| 문서2       | 0    | 1    | 1    | 1      | 1  | 0     | 0    |
| 문서3       | 1    | 0    | 0    | 0      | 0  | 1     | 1    |

- BoW를 기반으로 문서 간 유사도 계산 가능

---

## 5. Cosine Similarity (코사인 유사도)

두 벡터 간 코사인 각도를 이용하여 유사도를 측정
- 유클리드 거리와 달리 **벡터의 방향(패턴)** 만 고려
- 길이가 다른 문서 간 유사도를 공정하게 평가

특징:
- 각도 0° (같은 방향): 유사도 1 (최대)
- 각도 90° (직교): 유사도 0 (무관)
- 각도 180° (반대 방향): 유사도 -1 (완전 반대)

---

## 6. DTM의 한계
- **의미(Semantic)**를 고려하지 않음
- **희소 행렬(Sparse Matrix)** 문제
- **순서 정보** 무시

---

## 7. TF-IDF

단어의 **중요도**를 평가하는 방식
- 자주 등장하는 단어(TF)에 전체 문서 중 빈도(IDF)를 고려하여 중요도를 부여

### 계산 방법
- $\text{TF-IDF}(w, d) = TF(w, d) \times \log\left(\frac{N}{DF(w)}\right)$

예시:
- 문서 5개 중 단어 *"like"*가 2개 문서에서만 등장
- $\text{IDF}("like") = \ln(5/2) \approx 0.916$

---

## 8. LSA (Latent Semantic Analysis)

단어와 문서 간 **의미적 관계**를 파악하는 차원 축소 기법
- 핵심 기법: **특이값 분해 (SVD)**

### SVD (Singular Value Decomposition)
- 행렬 분해: $A = U \Sigma V^T$
- $U$: 단어 의미 방향 (좌측 고유벡터)
- $\Sigma$: 중요도 (특이값)
- $V^T$: 문서 의미 방향 (우측 고유벡터)

### Truncated SVD
- 주요한 의미를 나타내는 큰 특이값만 유지
- 덜 중요한 특이값은 버림 (차원 축소)

---

## 9. LDA (Latent Dirichlet Allocation)

문서에서 잠재된 **주제(Topic)**를 찾는 확률적 모델
- 문서 내 단어 분포를 기반으로 주제를 식별

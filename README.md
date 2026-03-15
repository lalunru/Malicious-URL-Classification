# 악성 URL 분류 (Malicious URL Classification)

LSTM 기반 혼합 신경망을 활용하여 URL을 **정상(Benign), 피싱(Phishing), 악성코드(Malware), 웹 변조(Defacement)** 4가지로 분류하는 딥러닝 프로젝트입니다.

---

## 프로젝트 개요

단순한 URL 문자열만으로는 탐지가 어렵다는 점에 착안하여,  
**URL 시퀀스(LSTM)** 와 **수작업 특성(Handcrafted Features)** 을 결합한  
**혼합 입력 신경망(Multi-Input Neural Network)** 을 설계했습니다.

---

## 모델 아키텍처
```
[URL 텍스트 시퀀스]          [수치형 특성 (9개)]
     ↓                              ↓
Embedding → LSTM(64)          Dense(32, ReLU)
     ↓                              ↓
          Concatenate(96)
               ↓
          Dense(64, ReLU)
               ↓
           Dropout(0.5)
               ↓
          Output(4, Softmax)
```

**추출 특성 (URLFeatureAnalyzer)**
- URL 엔트로피, 길이, 숫자 개수
- 파라미터 수, 서브도메인 수
- HTTP/HTTPS 포함 여부
- IP 주소 여부, `%20`, `@` 포함 여부

---

## 성능 결과

| 클래스 | Precision | Recall | F1-Score |
|--------|-----------|--------|----------|
| Benign | 0.87 | 0.94 | 0.90 |
| Defacement | 1.00 | 0.99 | 0.99 |
| Malware | 0.97 | 0.87 | 0.92 |
| Phishing | 0.86 | 0.87 | 0.86 |
| **Overall** | **0.92** | **0.92** | **0.92** |

---

## 데이터셋

[malicious_phish.csv](https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset) (Kaggle)

| 클래스 | 샘플 수 |
|--------|---------|
| Benign | 30,000 |
| Phishing | 30,000 |
| Defacement | 30,000 |
| Malware | 23,645 |

---

## 실행 환경

- Python 3.10 (Google Colab)
- TensorFlow / Keras, Pandas, NumPy, scikit-learn, Matplotlib, Seaborn

---

## 실행 방법

1. 데이터셋을 Google Drive에 업로드
2. Colab에서 노트북 실행
3. Google Drive 마운트 후 경로 설정

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lalunru/Malicious-URL-Classification/blob/main/(%EC%9D%B8%EA%B3%B5%EC%A7%80%EB%8A%A5)_%EC%95%85%EC%84%B1_URL_%EB%B6%84%EB%A5%98.ipynb)

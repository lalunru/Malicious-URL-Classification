# Malicious URL Classification

> 한국어 설명은 [여기](#korean)를 참고하세요.

A hybrid deep learning model that classifies URLs into 4 categories —
**Benign, Phishing, Malware, and Defacement** — achieving **~92% overall accuracy**.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lalunru/Malicious-URL-Classification/blob/main/(%EC%9D%B8%EA%B3%B5%EC%A7%80%EB%8A%A5)_%EC%95%85%EC%84%B1_URL_%EB%B6%84%EB%A5%98.ipynb)

---

## Overview

Raw URL strings alone are insufficient for reliable threat detection.
This project combines two complementary signals in a **multi-input neural network**:

- **LSTM branch** — treats the URL as a character sequence, learning structural patterns (subdomain nesting, path depth, suspicious suffixes)
- **Handcrafted feature branch** — encodes domain-knowledge signals a sequence model might overlook (entropy, IP address presence, special characters)

The two branches are concatenated and jointly trained end-to-end.

---

## Model Architecture

```
[URL text sequence]          [Handcrafted features (9)]
        │                              │
Embedding → LSTM(64)           Dense(32, ReLU)
        │                              │
        └──────── Concatenate(96) ─────┘
                        │
                 Dense(64, ReLU)
                        │
                  Dropout(0.5)
                        │
               Output(4, Softmax)
```

**Extracted features (URLFeatureAnalyzer)**

| Feature | Rationale |
|---|---|
| URL entropy | High entropy → random/obfuscated strings |
| URL length | Malicious URLs tend to be longer |
| Digit count | Excessive digits signal IP-based or generated URLs |
| Parameter count | Many params → potential injection or tracking |
| Subdomain depth | Deep nesting is a common phishing pattern |
| HTTP / HTTPS | Protocol as a weak signal |
| IP address presence | Direct-IP URLs bypass DNS — high-risk signal |
| `%20` presence | URL encoding often used to obscure payloads |
| `@` presence | `@` in URL redirects to attacker-controlled host |

---

## Results

| Class | Precision | Recall | F1-Score |
|---|---|---|---|
| Benign | 0.87 | 0.94 | 0.90 |
| Defacement | 1.00 | 0.99 | 0.99 |
| Malware | 0.97 | 0.87 | 0.92 |
| Phishing | 0.86 | 0.87 | 0.86 |
| **Overall** | **0.92** | **0.92** | **0.92** |

Defacement achieves near-perfect scores — its URL patterns (injected paths, foreign TLDs)
are highly distinctive. Phishing is the hardest class, as attackers actively mimic
legitimate URL structures.

---

## Dataset

[malicious_phish.csv](https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset) — Kaggle

| Class | Samples |
|---|---|
| Benign | 30,000 |
| Phishing | 30,000 |
| Defacement | 30,000 |
| Malware | 23,645 |

---

## Environment

- Python 3.10 (Google Colab)
- TensorFlow / Keras, Pandas, NumPy, scikit-learn, Matplotlib, Seaborn

---

## How to run

1. Upload `malicious_phish.csv` to Google Drive
2. Open the notebook in Colab (badge above)
3. Mount Google Drive and set the dataset path
4. Run all cells

---

## License

MIT — use freely for research and educational purposes.

---

<a name="korean"></a>
## 한국어 요약

LSTM 기반 혼합 신경망으로 URL을 정상, 피싱, 악성코드, 웹 변조 4가지로 분류하는 딥러닝 프로젝트입니다. 전체 정확도 약 92%를 달성했습니다.

**핵심 아이디어**
단순 URL 문자열만으로는 탐지가 어렵다는 점에 착안해, URL 시퀀스(LSTM)와 수작업 특성(엔트로피, IP 여부, 특수문자 등 9개)을 결합한 멀티 인풋 신경망을 설계했습니다.

**성과**
- Defacement: F1 0.99 (URL 패턴이 뚜렷해 거의 완벽 분류)
- Phishing: F1 0.86 (정상 URL을 모방하는 특성상 가장 어려운 클래스)
- 전체 Accuracy / Precision / Recall: 0.92

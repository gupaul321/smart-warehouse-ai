# smart-warehouse-ai

# 🏭 스마트 창고 AI — 물류 딜레이 예측 프로젝트

> 물류 창고 로봇 운영 데이터를 기반으로 **향후 30분간 평균 딜레이(분)** 를 예측하는 머신러닝 프로젝트

---

## 📌 프로젝트 개요

| 항목 | 내용 |
|------|------|
| **목표** | 창고 현재 상태 데이터로 30분 후 딜레이 예측 |
| **대회** | Dacon 스마트 창고 출고 지연 예측 AI 경진대회 |
| **평가지표** | MAE (Mean Absolute Error) |
| **팀원** | gupaul321, gupaul1 |

---

## 🎯 우리가 원하는 것

```
창고의 현재 상태 139가지 정보
        ↓
    [AI 모델]
        ↓
30분 후 딜레이가 몇 분 발생할지 예측
```

**예시:**
- 로봇 절반이 충전 중 + 주문 폭주 + 통로 혼잡
- → 모델: "이 상황이면 30분 후 딜레이 약 25분 예상"

이를 통해 **물류 담당자가 미리 대응**할 수 있게 합니다.

---

## 📁 데이터 구조

| 파일 | 크기 | 설명 |
|------|------|------|
| `train.csv` | 250,000행 × 94컬럼 | 학습 데이터 (정답 포함) |
| `test.csv` | 50,000행 × 93컬럼 | 예측할 데이터 |
| `layout_info.csv` | 300행 × 15컬럼 | 창고 구조 정보 |
| `sample_submission.csv` | 50,000행 × 2컬럼 | 제출 형식 |

### 주요 피처 종류

- 🤖 **로봇 상태** — 가동 중/충전 중/대기 중 로봇 수, 배터리 잔량
- 📦 **주문 상태** — 15분간 주문량, 긴급 주문 비율, 상품 종류 수
- 🚦 **혼잡도** — 혼잡도 점수, 막힌 통로 수, 고장 횟수
- 🌡️ **환경** — 창고 온도, 습도, 소음
- 🏭 **창고 구조** — 창고 타입, 통로 너비, 면적, 충전기 수

---

## 🔄 파이프라인

```
1. 데이터 로딩 & Merge (train + layout_info)
        ↓
2. 피처 엔지니어링 (108개 → 139개)
        ↓
3. 결측치 처리 (중앙값 / 0 채우기)
        ↓
4. 인코딩 (One-Hot Encoding)
        ↓
5. LightGBM 5-Fold 학습
        ↓
6. submission.csv 저장
```

---

## 📊 현재 성능

| 버전 | 변경 내용 | MAE |
|------|-----------|-----|
| v1 | LightGBM 기본 (n_estimators=5,000) | 6.7590 |
| v2 | n_estimators=10,000으로 증가 | **6.4164** |

> **MAE 6.41 의미:** 실제 딜레이가 10분일 때 모델은 약 ±6.41분 오차로 예측

---

## 🚀 추가 개선 계획

- [ ] `n_estimators` 20,000으로 증가
- [ ] XGBoost 앙상블
- [ ] CatBoost 추가
- [ ] Optuna 하이퍼파라미터 자동 튜닝
- [ ] 추가 피처 엔지니어링

---

## 🛠️ 사용 기술

![Python](https://img.shields.io/badge/Python-3.13-blue)
![LightGBM](https://img.shields.io/badge/LightGBM-latest-green)
![XGBoost](https://img.shields.io/badge/XGBoost-latest-orange)
![Pandas](https://img.shields.io/badge/Pandas-latest-lightgrey)
![Scikit--learn](https://img.shields.io/badge/Scikit--learn-latest-yellow)
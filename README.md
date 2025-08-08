# 👷‍♂️YOLO 기반 안전장비 탐지 모델 개발

## 1. 프로젝트 개요

이 프로젝트는 YOLO11 모델로 공사현장에서 사용하는 안전장비 객체를 실시간으로 감지하는 모델을 개발하는 것을 목표로 합니다. 탐지 대상은 `사람(person)`, `안전모(helmet)`, `안전대(belt)`, `안전조끼(vest)` 총 4개의 클래스입니다.

---

## 2. 모델 추가 훈련의 필요성

기존의 공개된 YOLO 모델은 다양한 객체를 인식하는 데는 뛰어나지만, 안전모, 안전대와 같이 특정 산업 현장에만 존재하는 객체에 대한 인식률은 낮습니다. 특히 안전대와 같이 복잡하고 작은 객체는 인식률이 낮습니다. 따라서 이 프로젝트는 다음과 같은 목표를 가지고 YOLO 모델을 **추가 훈련(Fine-tuning)** 했습니다.

- 정확도 향상: 건설 현장 특화 데이터를 학습시켜 안전장비 탐지 정확도를 극대화합니다.
- 일반화 성능 확보: 다양한 각도 등 여러 환경에서 안정적으로 작동하는 모델을 구축합니다.

---

## 3. 모델 정보

- **사용 모델:** YOLO11s
- **훈련 소스코드:** [Google Colab 링크](https://colab.research.google.com/drive/1H8QjBmVXj1mpswNyN161oyyto7Chrmv5?usp=sharing)

모델 훈련은 총 3차에 걸쳐 진행하며 성능을 최적화했습니다.
- **1차 훈련** 3개 클래스(person, belt, helmet)로 초기학습 진행.안전조끼(vest)를  안전대(belt)로  오인식하는 경우가 발생함.
- **2차 훈련** (🌟Best Model): 1차 데이터에 사람이 없는 공사장, 다양한 안전조끼 이미지 등을 추가하여 모델의 **오인식(False Positive)** 을 줄이는 데 집중. 이 단계에서 가장 우수한 성능의 모델을 확보.
- **3차 훈련** 2차 훈련 데이터셋을 활용해 에포크를 300으로 늘려 추가 성능 개선 시도.


---

## 4. 데이터셋 (Dataset)
기존에 공개된 데이터셋들을 수집해 필요한 데이터를 선별했습니다. 한국에서 사용할 것을 고려해 훈련에 사용한 이미지 대부분은 한국 공사현장 사진과 캐글에서  안전조끼 데이터를 수집해 사용했습니다. 


### 4.1. 데이터 출처
총 800장의 이미지를 선별해서 레이블링을 했습니다
- AI Hub: 530장
- Roboflow: 200장
- Kaggle: 75장

### 4.2. 클래스 정보
- person: 작업자 탐지를 위한 클래스
- helmet: 공사 현장 안전모 (색상 무관)
- belt: 추락 방지용 안전대
- vest: 시인성 확보를 위한 안전조끼 (형광색, 빨간색 등)


### 4.3. 데이터 분할
데이터 증강(Data Augmentation) 기법을 활용하여 데이터의 양을 늘렸습니다.

| 구분 | 이미지 수량 | 객체 수 |
|---|---|---|
| **Train** | 1,045장 | 5,174개 |
| **Valid** | 298장 | 1,547개 |
| **Test** | 151장 | 779개 |

---

## 5. 훈련 결과

### 5.1 그래프

가장 성능이 좋았던 2차 모델은 200 에포크 동안 약 1시간반 정도 훈련을 거쳤습니다.

#### 1) results
<img width="2400" height="1200" alt="Image" src="https://github.com/user-attachments/assets/85a71c06-5061-4db2-96c2-a5133c8977a1" />

- Precision (정밀도): 0.956
- Recall (재현율): 0.962
- mAP50: 0.977
- mAP50-95: 0.842

#### 2) confusion_matrix
<img width="700" height="2250" alt="Image" src="https://github.com/user-attachments/assets/eacd5379-89e8-44e2-b8a1-68ad5d455066" />

- person 0.97
- helmet 0.96
- belt : 0.94
- vest : 0.96


### 5.2 객체인식

<table>
<tr>
<th>1차 훈련 모델 영상에서 객체인식</th>
<th>2차 훈련모델 영상에서 겍체 인식</th>
</tr>
<tr>
<td><img src="https://github.com/user-attachments/assets/f0c6d182-a023-489e-9a1f-396b5ef03961" width="500"></td>
<td><img src="https://github.com/user-attachments/assets/e747a8d1-063c-4324-beda-b490e1469c09" width="500"></td>
</tr>
</table>

<table>
<tr>
<th>1차 훈련 모델 영상에서 객체인식</th>
<th>2차 훈련모델 영상에서 겍체 인식</th>
</tr>
<tr>
<td><img src="https://github.com/user-attachments/assets/bbc96661-851f-4ac5-9744-39b3f2c29e41" width="500"></td>
<td><img src="https://github.com/Yoo-SH-Alice/PPE_detection_YOLO/blob/main/ppe_detect_test.gif" width="500"></td>
</tr>
</table>

테스트 이미지 출처 pixabay, pexels







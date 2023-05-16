# 건설장비 오일 상태 분류

## 대회소개

- 건설 장비 내부 기계 부품의 마모 상태 및 윤활 성능을 오일 데이터 분석을 통해 확인하고, AI를 활용한 분류 모델 개발을 통해 적절한 교체 주기를 파악하고자 합니다.
<br><br>
## 상세내용

- 이번 경진 대회에서는 모델 학습시에는 주어진 모든 feature를 사용할 수 있으나, 진단 테스트시에는 제한된 일부 feature만 사용 가능합니다. 따라서 진단 환경에서 제한된 feature 만으로도 작동 오일의 상태를 분류할 수 있는 최적의 알고리즘을 만들어주세요.
<br><br>

## 데이터설명
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/ad9dcd19-f863-4a70-8151-e971542c7755" height="300px" width="300px">
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/d871caf3-3e92-45ec-9478-31e43d5b3fe6" height="300px" width="300px">

→ Train data에는 모든 Feature들이 존재하나, Test data에는 노란색의 Feature만이 존재하는 상황


<left><img src= "https://github.com/smart0515/Dacon-/assets/48974564/264699ed-0bbf-4e6b-a5c6-7914d0274364" height="300px" width="270px"></letf>

Target 분포 시각화
<br><br>

## Feature Engineering

**Scaler**

- Standard Scaler : 기존 변수의 범위를 정규 분포로 변환
- MinMax Scaler : 데이터의 값들을 0 ~ 1 사이의 값으로 변환
- Robust Scaler : Standard와 유사하지만 평균과 분산이 아닌 중위수와 사분위수를 사용<br><br>


**Encoder**

- Label encoder
- one-hot encoder <br><br>



**데이터 불균형 문제해결**

데이터 불균형 문제를 해결하기 위해서 **imbalanced-learn** 라이브러리를 활용하여 샘플링. 이때 다음과 같이 2가지 샘플링 기법을 사용<br><br>


[**Smote]**

대표적인 오버 샘플링 기법 중 하나

낮은 비율 클래스 데이터들의 최근접 이웃을 이용하여 새로운 데이터를 생성<br><br>


[**ENN / Tomek]**

ENN : CNN 방법은 최근접인 클래스 분포 데이터를 삭제하면서 샘플링하는 방법 (KNN과 비슷)

TomeK : 분포가 작은 클래스의 데이터에서 가장 가까운 분포가 높은 데이터의 위치를 찾는 방법
<br>
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/739c09f7-bfba-4e3a-ba0d-f6cfe4aaa047" height="200px" width="370px">
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/ec8367b9-6a9a-436e-ac96-42d27c9f5b6d" height="200px" width="370px">
<br><br>

오버, 언더 샘플링 기법이 결합된 2가지 기법을 사용<br><br>


[**Smote + TomeK]**

- shape : (20136, 18) → shape : (18475, 18)   **→**

[**Smote + ENN]**

- shape : (20136, 18) → shape : (25104, 18)   **→**

<img src= "https://github.com/smart0515/Dacon-/assets/48974564/fd29b764-390c-4e71-a2d2-25d0d02d9fb6" height="70px" width="150px"> <br>
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/672e2791-7579-4ee2-a248-087c63844677" height="70px" width="150px"> <br><br>



**이상치 제거**

- IQR 방식을 활용하여 이상치를 제거
<br><br>

## Model

### ML

- 사용할 모델 : LightGBM
- 사용할 기법 : SmoteENN + Label Encoder + Robust Scaler <br><br>

LightGBM 파라미터
<br>

<left><img src= "https://github.com/smart0515/Dacon-/assets/48974564/324c92f0-3f62-4fea-a6ac-81776a87de28" height="200px" width="300px"></left>
<br>

Tree 기반의 기계학습으로만 접근한 것이 아쉬움.  지식 증류 방식인 Teacher 모델로 학습 시키고 제한된 피처로 student 모델을 학습시키는 방법이 필요해 보인다.

### Knowledge distillation
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/191d5c6d-e93d-48b8-9529-5b6543fab7c2" height="500px" width="880px">
<br>

**Teacher와 Student model의 layer**

<img src= "https://github.com/smart0515/Dacon-/assets/48974564/2f3d3a83-4a7c-44df-ba9c-9dba89e8e6ec" height="450px" width="300px">
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/fe232565-d51f-4e49-97df-6af03978ba9e" height="450px" width="300px">

<br><br>


## Result

<img src= "https://github.com/smart0515/Dacon-/assets/48974564/60242ae4-aac3-4584-870e-2bfa8b39e623" height="50px" width="300px">
<br>
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/40562a58-28d3-4ece-b231-abbcc786ce00" height="50px" width="450px">

- Knowledge distillation방법이 Train과 Test시에 Feature의 수가 틀려도 사용할 수 있다는 것을 처음 확인할 수 있었고, 이후 이러한 경우가 있다면 더 발전시켜서 적용시켜보고자 한다.

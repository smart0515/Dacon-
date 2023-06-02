# 음식 검출 모델 개발
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/c6966cc1-401d-46e6-92ac-27bf3305d9b4" height="250px" width="700px">

## 대회소개
- 배경: 언택트 헬스케어 실생활 영양관리에 대한 니즈가 증가함에 따라 일상 속 식생활 영양관리가 필수적
- 설명: 양질의 음식 학습용 데이터를 활용해서 우리나라 국민의 다빈도 섭취 음식에 대해 label 된 이미지 데이터를 활용해 음식 사진 분류기 제작
- 내용: Object Detection과 Classification을 통해 사진에서 음식 위치를 파악하고 어떤 음식인지에 대한 정보를 제공
<br><br>

## 데이터 설명
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/fe7d340f-42cc-4731-82bd-f7178f957f6b" height="320px" width="700px">
<br><br>

## 모델 소개

**✔️Faster R-CNN이란?** <br><br>
기존의 Fast R-CNN의 후보 영역 추출을 위해 사용되는 Selective Search 알고리즘의 병목 현상을 <br>
Region Proposal Network(RPN) 알고리즘을 통해 해결한 모델. 즉, RPN + Fast R-CNN

<br><br>

**✔️Faster R-CNN 동작 과정** <br>
1. 원본 이미지를 pre-trained 된 CNN모델에 입력하여 feature map 획득
2. feature map은 RPN에 전달 -> region proposal 산출
3. Rol pooling을 수행하여 고정된 크기의 feature map 획득
4. Fast R-CNN 모델에 고정된 크기의 feature map을 입력하여 Classification과 Bounding box regression 수행 <br>
![image](https://github.com/smart0515/Dacon-/assets/48974564/68c78d79-cb84-43a6-b514-df1d02a24a33)

<br><br>

✔️**RPN(Region Proposal Network)이란?**

- 원본 이미지에서 region proposals 대하여 class score를 매기고 bounding box confidence를 출력
    
    → 기존과 달리 Region proposals를 추출하기 위해 scale과 aspect ratio가 다른 Anchor Box사용
    
- class score에 따라 상위 N개의 region proposals만 추출하고 Non maximum suppression을 적용하여 최적의 region proposals만 Fast R-CNN에 전달

<br><br>

✔️**RPN 동작 과정**

1) 원본 이미지를 pre-trained된 VGG 모델에 입력하여 feature map을 얻음
2) feature map에 대하여 padding을 추가하여 3x3 conv 연산
3) class score를 매기기 위해 feature map에 대해 1x1 conv 연산
     - (RPN에서는 후보 영역이 어떤 class에 해당하는지는 분류하지 않고 객체가 포함되어 있는지 여부만 분류)
     - channel 수는 2(object 여부) x 9(anchor box 개수)

4) bounding box regressor를 얻기 위해  feature map에 대해 1x1 conv 연산
     - channel 수는 4(bounding box regressor) x 9(anchor box 개수)
<br><br>


#### [RPN 결과] <br>
![image](https://github.com/smart0515/Dacon-/assets/48974564/3a89a8a0-3dd5-465b-99a2-519ad1cb42af)

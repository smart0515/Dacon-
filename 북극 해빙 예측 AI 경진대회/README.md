# 북극 해빙 예측 AI 경진대회

## 대회소개

**|시계열|비전|** <br>
지구 온난화로 인해 북극 해빙의 면적이 줄어들고 있음<br>
1978년부터 관측된 북극 해빙의 변화를 이용하여 **주별** 해빙 면적 변화 예측<br>
- 과거 관측된 해빙 데이터를 통해 앞으로 변화할 해빙을 예측

## 상세내용
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/89467694-07a8-4927-afb8-25199ec11a9a" height="200px" width="500px">


## 데이터설명
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/fbabfa63-77ff-4e44-839a-b8f022b99ca3" height="400px" width="400px">
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/4aef6e1a-6aff-40c3-8296-d3f076458d74" height="200px" width="900px"> <br><br>
각 파일은 5개 채널의 배열로 구성 되어있습니다.<br>
1. 해빙 농도(0~250)<br>
2. 북극점 (위성 관측 불가 영역)<br>
3. 해안선 마스크<br>
4. 육지 마스크<br>
5. 결측값<br><br>

<img src= "https://github.com/smart0515/Dacon-/assets/48974564/d8d7137a-731c-4868-9eab-a14ac6176400" height="250px" width="900px">

## 모델 선정
- 각각이 이미지 데이터인데 시계열적 특성을 지님 -> ConvLSTM
- 본 데이터는 고정된 길이의 데이터로 Seq2Seq 구조를 추가함

<br><br>

### Seq2Seq
- RNN은 출력이 바로 이전 입력까지만 고려하고, 경사소실이 일어나기 때문에 정확도가 떨어짐.
- 따라서 LSTM을 사용하여 경사 소실을 방지.
- 이를 Encoder와 Decoder의 형태로 모델화 한 것이 seq2seq이다.
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/2a8f6075-ac64-40dc-ba18-ccfea664ddc1" height="250px" width="1000px">

<br><br>

### ConvLSTM + Seq2Seq
자연어 처리     ->    이미지 처리
- Seq2seq는 고정길이의 인코더를 통해 하나의 context 벡터를 생성한다.
- 생성된 context 벡터를 디코딩하며 다음 올 값을 예측한다.
- 여기서는 자연어가 아닌 이미지를 처리할 것이기 때문에 LSTM 셀이 아닌 ConvLSTM셀을 사용한다.
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/e0a8f5f4-a225-4d67-94aa-daa6ef8a754d" height="250px" width="1000px">

<br><br>

### ConvLSTM2D 실행 예시
<img src= "https://github.com/smart0515/Dacon-/assets/48974564/5b752657-b7c4-4f77-8362-78d7d3ba3005" height="450px" width="1000px">




















# 2023 전력사용량 예측 AI 경진대회

- 심사기준 : SMAPE (Symmetric Mean Absolute Percentage Error)

- 데이터
  - train.csv
  
    train 데이터 : 100개 건물들의 2022년 06월 01일부터 2022년 08월 24일까지의 데이터
    일시별 기온, 강수량, 풍속, 습도, 일조, 일사 정보 포함
    전력사용량(kWh) 포함
  
  
  - building_info.csv
  
    100개 건물 정보
    건물 번호, 건물 유형, 연면적, 냉방 면적, 태양광 용량, ESS 저장 용량, PCS 용량
  
  
  - test.csv
  
    test 데이터 : 100개 건물들의 2022년 08월 25일부터 2022년 08월 31일까지의 데이터
    일시별 기온, 강수량, 풍속, 습도의 예보 정보
  
  
  - sample_submission.csv
  
    제출을 위한 양식
    100개 건물들의 2022년 08월 25일부터 2022년 08월 31일까지의 전력사용량(kWh)을 예측
    num_date_time은 건물번호와 시간으로 구성된 ID
    해당 ID에 맞춰 전력사용량 예측값을 answer 컬럼에 기입해야 함

# 2020-2-Stock-Recommendation
> Mirae Asset Big Data Festival :: 주식 거래종목 TOP3 예측 프로젝트
> Feature Engineering + Recommendation Model with Item2Vec + LGBM Regressor with Bayesian Opimization

Members: 김수정, 김지환, 안지훈

### 1. Feature Engineering

- 기존 feature를 이용하여 새로운 feature 생성
(ex. 전일비, 등락률, 당일등락량, 증가율, 거래중지이력)

- 시가총액 관련 정보를 크롤링하여 새로운 feature 생성
크롤링 출처: https://github.com/FinanceData/marcap, https://www.krx.co.kr
(ex. 시가총액, 외국인지분율(%), 코스닥순위)

- 지수평활법을 이용하여 새로운 예측 feature 생성
(ex. 매수고객수_ES3, 매수고객수_ES6)


### 2. Model 1 : Recommendation Model with Item2Vec
- 그룹별 매수 종목 시퀀스 생성
그룹별로 2019년 7월부터 2020년 6월까지 매수한 주식 종목을 순서대로 뽑아서 시퀀스 생성

- word2vec 모델을 이용하여 주식 종목 벡터화
window size와 vector size를 바꿔가며 실험하여 최적의 값 찾음 (window = 1, size = 400)

- matrix multiplication
count matrix는 2 dimension matrix로, 고객 그룹별 주식 종목별 매수 고객수 총합을 나타냄
similarity matrix는 2 dimension matrix로, word2vec을 통해 얻은 주식 종목별 벡터를 이용해 계산한 cosine similarity값을 나타냄
count matrix와 similarity matrix를 matrix multiplication 하여 주식 종목별 score 계산
가장 높은 score를 기록한 주식 종목 TOP3를 추출

- model evaluation
2020년 6월 데이터를 validation data로 하여 예측 결과와 비교
예측력이 낮은 8, 12, 20, 32, 41, 42번 group에 대해 다른 분석방법 적용


### 3. Model 2 : LGBM Regressor with Bayesian Opimization
- data preprocess
Drop useless or flawed columns
Convert data type to category
Scale data

- LGBM Regressor 적용


### 4. Final Results
Group 8, 12, 20, 32, 41, 42 : result of LGBM Regressor
the other groups : result of Item2Vec

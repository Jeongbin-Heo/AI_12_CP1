# 깜빠구니
* **개요** : 식료품 재구매 요인 분석 및 맞춤 식료품 추천 서비스 개발
* **진행 기간** : 2022. 06. 24 ~ 2022. 07. 07
* **사용 스킬** : `etc`


### &nbsp;

## *I. 프로젝트 개요*
### 프로젝트 진행 배경<img width="923" alt="스크린샷 2022-09-11 오후 7 05 46" src="https://user-images.githubusercontent.com/97662174/189521929-8c63a4b0-254b-409f-80d4-bf30acef7b45.png">

* 온라인쇼핑 시장에서 음/식료품의 거래가 차지하는 비율이 제일 크며 그 증가폭 또한 다른 상품군과 비교하여 높은 편
* 고객들의 구매 요인을 파악하여 음/식료품의 구매를 촉진시키면 매출 상승에 큰 기여를 할 것으로 예상
### 프로젝트 진행 방향
* 고객의 음/식료품 구매 이력 데이터를 시각화하여 재구매에 영향을 미치는 요인에 대한 가설 설정
* 고객의 재구매 여부를 예측하는 분류 모델 설계 및 해석을 통해 이전에 설정한 가설과 비교하며 실제 고객의 재구매에 영향을 미치는 요인 분석
* 재구매 요인을 고려하여 제품 추천 모델을 설계하고 해당 모델을 이용하여 고객별 맞춤 제품 추천 서비스 구현
### 사용 데이터
* **데이터 출처** : [Instacart Market Basket Analysis](https://www.kaggle.com/c/instacart-market-basket-analysis)
<img width="451" alt="스크린샷 2022-09-11 오후 7 16 08" src="https://user-images.githubusercontent.com/97662174/189522303-7557253b-3648-4023-b9cd-3bc674eb92f2.png">

|Feature|Description|
|:---:|---|
|order_id|주문 고유 아이디|
|user_id|소비자 고유 아이디|
|product_id|제품 고유 아이디|
|eval_set|소비자 군(prior/train_test)|
|order_number|주문번호|
|order_dow|주문 요일|
|order_hour_of_day|주문 시간대|
|days_since_prior_order|재주문까지 걸린 시간|
|add_to_cart_order|장바구니에 담은 제품의 개수|
|reordered|제주문 여부(0 or 1)|
|product_name|제품 이름|
|aisle_id|제품 상세 분류 고유 아이디|
|department_id|제품 분류 고유 아이디|
|aisle|제품 상세 분류|
|department|제품 분류|


### &nbsp;

## *II. 데이터 시각화를 통한 가설 설정*
### 데이터 시각화
### 가설 설정

### &nbsp;

## *III. 재구매 예측 모델 설계 및 해석*
### Feature Engineering
### 기본 모델 설계
### Undersampling
### 하이퍼 파라미터 튜닝
### 모델 평가 및 
### 모델 결과 해석

### &nbsp;

## *IV. 제품 추천 모델 설계 및 서비스 구현*
### 제품 추천 모델 설계
### 서비스 구현

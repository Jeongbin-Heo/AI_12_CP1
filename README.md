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
* 고객의 음/식료품 구매 이력 데이터를 시각화하여 음/식료품 재구매 동향 분석
* 고객의 재구매 여부를 예측하는 분류 모델 설계 및 해석을 통해 재구매에 영향을 미치는 요인 분석
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
<img width="505" alt="스크린샷 2022-09-12 오후 10 21 02" src="https://user-images.githubusercontent.com/97662174/189667072-acf1118e-728c-47b8-ab16-440a12a86ba8.png">

**제품 카테고리별 재주문율**  
-> 유제품, 달걀, 농산품, 음료, 빵, 애완용 간식 카테고리 제품의 재구매율이 높음

<img width="375" alt="스크린샷 2022-09-12 오후 10 22 15" src="https://user-images.githubusercontent.com/97662174/189667102-f40d4435-9ddd-4080-a6dc-f91e663fac99.png">

**주문 요일별 재구매율**  
-> 대체로 월초의 재구매율이 높으며 월말로 갈수록 줄어드는 경향을 보임  

<img width="519" alt="스크린샷 2022-09-12 오후 10 22 43" src="https://user-images.githubusercontent.com/97662174/189667524-980af762-14b8-4583-9016-ebb460c0c2d8.png">

**장바구니 추가 순서별 재구매율**  
-> 장바구니에 먼저 담은 제품의 재구매율이 높고 나중에 담은 제품일수록 재구매율이 감소하는 경향을 보임

### 시각화 결과 해석
> **제품 카테고리별 재구매율**
>> 재구매율이 높은 제품 카테고리는 유제품, 달걀, 농산품, 음료 등 요리 재료로 쓰이거나 일상적으로 자주 섭취하는 제품이라는 것을 알 수 있다. 

> **주문 요일별 재구매율**
>> 극명한 차이를 보이지는 않으나 웚초의 재구매율이 높으므로 달이 바뀌면 다시 주문을 하는 경우가 많다는 것을 알 수 있다.

> **장바구니 추가 순서별 재구매율**
>> 자주 구매를 하는 제품들은 주문할때마다 장바구니에 먼저 담고 다른 제품들을 추가적으로 담는 것을 알 수 있다.

### &nbsp;

## *III. 재구매 예측 모델 설계 및 해석*
### Feature Engineering
<img width="900" alt="스크린샷 2022-09-11 오후 7 48 53" src="https://user-images.githubusercontent.com/97662174/189523411-9e929669-6b25-4c68-8225-6d033b9f7624.png">

|Feature|Description|
|:---:|---|
|u_order_num|고객의 해당 쇼핑몰에서의 주문 횟수|
|u_avg_basket|고객이 주문마다 장바구니에 담은 평균 제품의 수|
|u_most_dow|고객이 일주일 중 가장 주문을 많이 한 요일|
|u_most_hod|고객이 하루 중 가장 주문을 많이 한 시간대|
|u_reorder|고객의 해당 쇼핑몰에서의 재주문율|
|u_avg_gap|고객의 평균 주문 주기|
|u_product_num|고객이 해당 쇼핑몰에서 주문한 제품의 총합|
|p_order_num|모든 고객에 대한 특정 제품 주문 횟수|
|p_reorder|모든 고객에 대한 특정 제품 재주문율|
|p_avg_basketposition|모든 고객이 특정 제품을 장바구니에 담은 평균 순서|
|up_order_num|고객의 특정 제품 주문 횟수|
|up_reorder|고객의 특정 제품 재주문율|
|up_avg_basketposition|고객이 특정 제품을 장바구니에 담은 평균 순서|
|**reordered**|**재주문 여부 ( 0 or 1 ) [target]**|

### 기본 모델 설계
* 모델을 학습시키기 위한 학습 셋(80%)과 일반화 성능을 검증할 테스트 셋(20%)으로 데이터를 분리
* **기본 모델** : `XGBoost`
* **평가 지표** : `F1 Score`

|Dataset|기본 모델|
|:---:|:---:|
|Train set|0.2010|
|Test set|0.1909|

* 기본 모델의 경우 테스트 셋에 대해 매우 저조한 일반화 성능을 나타냄
* 데이터에 문제가 있다고 판단하여 데이터 분석 실시

### Undersampling

<img width="446" alt="스크린샷 2022-09-12 오후 8 52 28" src="https://user-images.githubusercontent.com/97662174/189654023-a0cc745d-ac6a-4bf3-b225-dc027e49d347.png">

* target 값인 `reordered` 클래스 비율 확인 결과 매우 편향된 데이터라는 것을 확인
* 데이터 불균형이 심할 경우 모델의 학습 과정에서 일반화 성능을 이끌어내기 힘드므로 균형을 맞춰주어야 함

<img width="557" alt="스크린샷 2022-09-12 오후 8 55 47" src="https://user-images.githubusercontent.com/97662174/189654310-26f1c5f1-f88e-4cce-ae1f-38269975b0c9.png">

<img width="436" alt="스크린샷 2022-09-12 오후 8 54 51" src="https://user-images.githubusercontent.com/97662174/189654509-cc9d3bd6-b70c-42ad-8820-6da98bc6e1ee.png">

* Undersampling 기법 중 하나인 Nearmiss 알고리즘을 사용하여 target 클래스의 균형을 맞춰줌

### 하이퍼 파라미터 튜닝
* **기본 모델** : `Decision Tree` `Random Forest` `XGBoost`
* 트레인 셋에 대해 **RandomizedSearchCV**를 통해 하이퍼파라미터를 튜닝하며 학습

|HyperParameter|Decision Tree|Random Forest|XGBoost|
|:---:|:---:|:---:|:---:|
|1|max_depth|n_estimators|n_estimators|
|2|min_samples_leaf|max_depth|max_depth|
|3|max_features|min_samples_leaf|learning_rate|
|4||max_features||

### 모델 평가 및 선택
* **평가 지표** : `F1 Score`
* 테스트 셋에 대해 Score가 가장 높은 모델을 일반화 성능이 가장 우수한 최종 모델로 선정

|Dataset|Decision Tree|Random Forest|XGBoost|
|:---:|:---:|:---:|:---:|
|Train set|0.7870|0.8069|0.7936|
|Test set|0.7880|**0.8061**|0.7958|

-> **Random Forest**를 최종 모델로 선정
### 모델 결과 해석
최종 모델인 Random Forest 모델의 **특성 중요도**를 확인

<img width="942" alt="스크린샷 2022-09-12 오후 9 07 58" src="https://user-images.githubusercontent.com/97662174/189656584-3cd28879-5582-4896-b1f6-45ec5d0be88f.png">


> **가장 모델에 영향을 크게 미치는 특성**
>> `p_order_num` : 모든 고객에 대한 특정 제품 주문 횟수  
>> `up_order_num` : 고객의 특정 제품 주문 횟수  
>> `up_reorder` : 고객의 특정 제품 재주문율  
>> `p_reorder` : 모든 고객에 대한 특정 제품 재주문율  
>> 나머지 특성은 그 중요도가 0.1 이하로 매우 낮아 영향을 거의 미치지 않음  

> **특성 중요도 기반 재구매 요인 분석**  
>> `p_order_num` `p_reorder` : 모든 고객이 구매를 많이 하는 인기 상품을 구매하는 경우가 많음  
>> `up_order_num` `up_reorder` 이전에 주문을 했던 제품을 재구매하는 경우가 많음

> **재구매 요인 분석 기반 제품 추천 모델 설계 계획**  
>> 고객의 장바구니 이력을 분석하여 이전에 구매를 많이 한 제품과 연관이 있는 인기 제품을 추천하는 모델 

### &nbsp;

## *IV. 제품 추천 모델 설계 및 서비스 구현*
### 제품 추천 모델 설계
### 서비스 구현

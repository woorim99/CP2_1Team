# E-Commerce 웹 로그 데이터를 활용한 추천 시스템

## 팀 구성 및 역할
1. **이우림**: 데이터 분석 및 시각화, 아이템 기반 협업 필터링, 추천시스템 구현
2. **이상빈**: 데이터 전처리, 분석 및 시각화, 모델링

## 프로젝트 배경
1. 사이트를 도출하는 과정에서 '이커머스의 다양한 상품 종류 중에서 고객의 니즈에 맞춰 원하는 상품을 추천하면 판매량이 늘어나지 않을까' 라는 생각에서 추천시스템 개발
2. 데이터 분석과 모델링을 통해 추천시스템을 개발하여 소비자에게 원하는 물건들을 많이 노출시켜 구매욕구를 상승시키는 것이 목적

## 데이터 소개(출처: kaggle)
1. **event_time**: 이벤트가 일어난 시각
2. **event_type**: 이벤트 종류(view, cart, purchase)
3. **product_id**: 제품 고유 id
4. **category_id**: 제품 카테고리 고유 id
5. **category_code**: 제품 카테고리 명
6. **brand**: 제품 브랜드 명
7. **price**: 제품 가격
8. **user_id**: 고객 고유 id
9. **user_session**: 고객 세션

## 데이터 시각화 및 분석 - cp2.ipynb
### 1. 대분류 카테고리 별 전환율
![image](https://user-images.githubusercontent.com/64140376/185625223-fd476586-7446-4a51-a943-077b6dee6c3d.png)
> - electronics는 높은 전환율, apparel은 낮은 전환율
> - apparel 카테고리의 전환율을 높일 수 있는 방안을 탐색하기 위해 2번과 같이 apparel 카테고리의 가격 분석 진행

### 2. apparel 카테고리의 브랜드 별 전환율
![image](https://user-images.githubusercontent.com/64140376/185626026-01406639-7355-4cd5-8b57-21c8cec9d96b.png)
> - 빨간색으로 표시된 Fassen과 rooman이라는 브랜드는 전환율이 높은편, 노란색으로 표시된 nike, adidas, milavitsa라는 브랜드는 전환율이 매우 낮은 편
> - 이 브랜드들의 전환율이 낮은 이유를 파악하기 위해 3번과 같이 브랜드 별 평균 가격 비교 진행

### 3. apparel 카테고리의 브랜드 별 평균 가격
![image](https://user-images.githubusercontent.com/64140376/185626439-39845dae-795a-4fae-af20-cc2eb5b55b71.png)
> - 전환율이 낮은 브랜드 였던 nike와 adidas는 총 86개의 브랜드 중 가격 순위가 20위와 29위로 평균 가격이 높은 편
>> - **할인 쿠폰 발급이나 행사를 통해 전환율을 높이자!**
> - 전환율이 낮은 브랜드 였던 milavitsa는 총 86개의 브랜드 중 평균 가격 순위가 80위로 가격이 상당히 낮은 편
>> - **가격이 저렴함에도 전환율 낮음 -> 가격이 너무 저렴해도 소비자들의 구매 욕구를 반감시킬 수 있다고 보일 수도 있지만 보다 정확한 이유를 파악하기 위해서 시장 조사를 통해 소비자들의 이야기를 들어보자!**

### 4. 카테고리 별 객단가
![image](https://user-images.githubusercontent.com/64140376/185627494-a3291eb0-17b5-4936-bf72-892caac4bd1f.png)
> - 옷이나 문구류 등의 객단가가 낮은 편
>> - 단가가 낮으므로 객단가가 낮은 것이 당연하지만 만약 객단가를 높이는 방안을 탐색해 본다면 묶음 상품을 좀 더 저렴하게 판매하거나 무료배송료 금액 기준을 설정하는 방법 등이 존재

## 아이템 기반 협업 필터링 - cp2_recommend_system_baseline.ipynb
1. 아이템 간 유사도를 활용한 협업 필터링을 진행하기 위해서 이벤트 타입 별로 view는 1점, cart는 2점, purchase는 3점 부여
2. 제품들의 코사인 유사도 측정

## XGBoost 모델링
1. Cart/View에서 고객이 제품을 구매할 것인지 예측하는 모델
2. 특성 공학
> - category_code_level1: 메인 카테고리
> - category_code_level2: 서브 카테고리
> - activity_count: user_session을 통한 활동 수 카운트
> - is_purchase: cart/view의 품목 구매 여부

## 추천 시스템 - cp2_recommend_system.ipynb
![image](https://user-images.githubusercontent.com/64140376/185628037-686fc760-5dc8-4990-82e7-24e69b22d5f0.png)
1. 고객 아이디를 입력하게 되면 고객이 가장 최근 조회한 아이템과 유사한 5개의 아이템들을 아이템 기반 협업 필터링을 통해 추출
2. 그 5개의 아이템들을 XGBoost 모델에 넣어 고객이 그 아이템을 구매할지 예측
3. 만약 고객이 그 아이템을 구매할 것이라고 예측한다면 추천 우선순위가 높아지고 그 아이템을 구매할 것이라고 예측하지 않는다면 추천 우선순위가 뒤로 밀림

> **결과사진**
> 
>![image](https://user-images.githubusercontent.com/64140376/185628390-a346c73a-fbf6-4a35-abf6-417b186d23b8.png)




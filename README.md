# 식단 추천 시스템을 위한 음식 다중분류모델 개발
### 새로운 메뉴들을 특징에 따라 라벨링 하는 모델 작업 (누비랩 협업)

## 프로젝트 전체 목차
#### 1. 서론
  - 주제 선정 이유 
  - 데이터 특성
#### 2. 탐색적 자료 분석
#### 3. 음식 메뉴 다중 분류 (대분류 7 / 중분류 26)  
  - 분류 모델 선택
  - 모델 학습 및 성과 측정
  - 결과 분석
#### 4. 한계점 및 추후 발전방향

---
## 프로젝트 요약
### 주제 선정 이유 및 데이터 특성
<img width="800" alt="001" src="https://user-images.githubusercontent.com/91524805/176125875-ac076ef1-0742-4586-8b1d-f8eae9616d22.png"> 
<img width="800" alt="002" src="https://user-images.githubusercontent.com/91524805/176126011-08d41863-7e46-4e84-ad84-1fd8b2461312.png">

### 분류 모델 선택
* **모델 선정**: KoBERT, 새로운 단어 조합이 많은 복잡한 신규 음식명도 분류할 수 있는 모델로 선정
* **분류 프로세스**: 기존 라벨링 자료를 모델에 학습 시키고 결과 검증 → 신규 데이터 분류 후 검증 
* **분류 기준**: 대분류 7종류, 중분류 26종류로 구분 (누비랩 내부 기준)
<img width="800" alt="003" src="https://user-images.githubusercontent.com/91524805/176126261-ae33fc7f-d128-4918-b4dc-8bc0b22dc8d5.png">

### 모델 학습 및 성과 측정
<img width="800" alt="004" src="https://user-images.githubusercontent.com/91524805/176126481-17a3704f-2d04-49f2-9db0-56e61a27c86c.png">  

* 기존 라벨링 자료(1855개) 학습한 모델 사용 (`학습acc 0.95` , `검증acc 0.89`) → **신규 1만 개** 분류  
  → 오분류 항목을 목록화 → 모델 재학습 (`학습acc 0.98`, `검증acc 0.96`, 최종 모델) → **전체 5.3만 개** 분류

### 결과 분석 
* 신규 데이터는 정답 라벨이 없으므로 분류 후 결과를 다양한 방식으로 여러 번 **교차 검증 및 시각화**
    - **클러스터링**(K-Means): 분류된 항목 내에서 그룹화, 어떤 기준으로 묶였는지 검토
    - **키워드 필터링**(KoNLPy Okt): 한글 명사 추출하여 빈도 계산, 주요 키워드 포함/미포함 분류 검토
    - **Word Cloud**: 분류된 음식명 빈도수에 따라 시각화 하여 오분류 키워드 파악
    
<img width="800" alt="005" src="https://user-images.githubusercontent.com/91524805/176126543-1c8ec986-8fde-47c5-a798-165a215bebf1.png">
<img width="800" alt="006" src="https://user-images.githubusercontent.com/91524805/176126678-36a6f349-0f85-4577-99e2-64f29e023e4e.png">
<img width="800" alt="007" src="https://user-images.githubusercontent.com/91524805/176126728-3b7adcbe-b9fb-4ee4-9711-806098859289.png">

* 총 삭제할 데이터 9,603 (18.2 %, 오탈자 및 추가 데이터 정제가 필요한 경우로 삭제하는 경우 12% 포함)
* **총 4.3만 데이터 대분류, 중분류 완료**, 대분류가 다른 경우: **515건 (1.19 %)**

### 한계점 및 추후 발전방향
* BERT 모델이 OOV처리에 효과적이고, 다양한 새로운 항목을 분류할 수 있어서 선택했습니다. 그대신 분류된 항목 중 잘못 분류된 항목이 있는지 검토가 아주 어려웠는데, 미학습 된 항목을 분류할 수 없다고 나누는 편이 더 정확도를 높일 수도 있겠다고 추후에 생각이 들었습니다.
* 딥러닝 모델이 학습하지 않은 새로운 음식명이 들어왔을 때 어떻게 분류할 지 이해하기가 어렵습니다. 하지만 분류가 복잡한 항목(예: 반찬, 디저트)은 기준을 세우기 힘든 경우가 많은데 KoBERT모델로 분류를 해보고 결과를 확인하며 인사이트를 얻는 방향으로 사용하면 좋을 것 같습니다.

---
## 참고자료
* [BERT 동작구조](https://ebbnflow.tistory.com/m/151)
* [BERT계열 특징비교](https://littlefoxdiary.tistory.com/m/81)
* [Word Cloud](https://towardsdatascience.com/generate-meaningful-word-clouds-in-python-5b85f5668eeb)
* [Text Data Analysis](https://towardsdatascience.com/advanced-visualisations-for-text-data-analysis-fc8add8796e2)

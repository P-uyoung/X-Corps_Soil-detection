<h2 align="center">X-Corps_Soil-detection</h2>

> **서울대학교 실전문제연구단 팀프로젝트**

> **토양의 질(soil organic carbon, SOC) 예측**

> **프로젝트 기간 : 2022.05.31 ~ 2022.11.31 (6개월)**

<!-- >https://go-quality.dev   -->
<br/>

## **목차** 
<b>

- [요약](#요약)
- [기술 및 도구](#기술-및-도구)
- [구현](#구현)
  - [프로세스](#1-프로세스)
  - [데이터 수집](#2-데이터-수집)
  - [모델링](#3-모델링)
- [트러블 슈팅](#트러블-슈팅)
- [커밋 히스토리](#-커밋-히스토리)
</b>
<br/>

## **요약**
- (input)MSI, SWHC, ST을 통해 (output)SOC 함량 예측.
- input features를 제거 및 추가하여 기존 논문의 정확도에서 **"60% (0.52->0.83 결정계수) 성능이 향상된"** 모델링 함.
- 토양 황폐화 상태를 SOC 모니터링을 통해 점검하고 예방할 수 있음.
- 논문 작성 중. (3월초 투고예정)
<br/>

## **기술 및 도구**
  <b>- 데이터 : </b>
  <span><img src="https://img.shields.io/badge/Python-05122A?style=flat-square&logo=python"/></span>
  <span><img src="https://img.shields.io/badge/Qgis-589632?style=flat-square&logo=Qgis&logoColor=white"></span>
  <span><img src="https://img.shields.io/badge/Snap-071D49?style=flat-square&logo=Snap&logoColor=white"/></span>
  <span><img src="https://img.shields.io/badge/Google%20Cloud-4285F4?style=flat-square&logo=google-cloud&logoColor=white"/></span>
  
  <br/>
  <b>- 모델링 : </b>
  <span><img src="https://img.shields.io/badge/Pytorch-EE4C2C?style=flat-square&logo=PyTorch&logoColor=white"></span>
  <br/>
  <b>- 논문 : </b>
  <span><img src="https://img.shields.io/badge/-Latex-008080?style=flat&logo=LaTex"></span>

<br/>


## **구현**
<details>
<summary><b>구현 설명 펼치기</b></summary>
<div markdown="1">

### 1. 프로세스
![](https://github.com/P-uyoung/X-Corps_Soil-detection/blob/main/figure/process.png)

### 2. 데이터 수집
- k-means clustering을 통해 SOC 함량의 variance를 고려한 토양 채취 실험을 계획함. [코드 확인](https://github.com/P-uyoung/X-Corps_Soil-detection/tree/main/k-means)
- SNAP, QGIS 프로그램을 이용해 해당 지역의 MSI 데이터를 얻음 (input feature 1)  
- 간단한 실험을 통해 SWHC, ST 데이터를 얻음 (additional input features)  
- [수집한 데이터 자료](https://github.com/Integerous/goQuality/blob/b587bbff4dce02e3bec4f4787151a9b6fa326319/frontend/src/components/PostInput.vue#L67)

### 3. 모델링
- 모델링 결과
![](https://github.com/P-uyoung/X-Corps_Soil-detection/blob/main/figure/result.png)  
- [코드 확인](https://github.com/P-uyoung/X-Corps_Soil-detection/tree/main/uyoung_model)  
- 상세 설명 
  <details>
  <summary>상세 설명 </summary>
  <div markdown="1">

  1. 10m resolution  
    2. 20m resolution  
    3. Feature  
        1. 1단계 - 기존 연구 방법대로
            1. B2 ~ B12
            2. NDVI
            3. BSI
        2. 2단계 
            1. B2 ~ B12
            2. NDVI
            3. BSI
            4. 토양에서 직접 추출한 features (Approach1 : hybrid remote sensing)
                1. SWHC (Soil Water Holing Capacity)
                2. Sand (%)
            5. SAR (synthetic aperture radar) (Approach2 : full remote sensing)
                1. before rain
                2. after rain
    4. Label
        1. SOC (Soil Organic Carbon)
    5. Normalize
        1. MSI의 각 band를 StandardScaler
    6. Modeling
        1. 1단계: 20m resolution data를 가지고 다음 세 가지 method로 모델링한다.
            1. SVM (Support Vector Machine)
            2. PLSR (Partial Least Squares Regression)
            3. RF (Random Forest)
        2. 2단계
            1. 1단계에서 제일 잘 fit 되는 모델로 다음과 같이 네 번의 모델링을 한다.

            <img width="398" alt="image" src="https://user-images.githubusercontent.com/63593428/199702219-f815e88a-d5fa-43b0-b08d-529329d61ace.png">

        3. train dataset : test dataset =  8:2
        4. Evaluation
            1. R-squared

  </div>
  </details>
    

</div>
</details>

</br>

## 트러블 슈팅
### 1. 훈련데이터가 적어서 모델이 과적합 됨.
- 문제상황 : Train의 성능 , test의 성능이 ~으로, 과적합 모델임. 
![](https://github.com/P-uyoung/X-Corps_Soil-detection/blob/main/figure/overfitting.png)  
<details>
<summary><b>기존 코드</b></summary>
<div markdown="1">

~~~python


~~~

</div>
</details>

- 해결방법 : 데이터 증강을 통해 훈련데이터 수를 증가시킴.
![](https://github.com/P-uyoung/X-Corps_Soil-detection/blob/main/figure/final.png)
<details>
<summary><b>수정 코드</b></summary>
<div markdown="1">

~~~python


~~~

</div>
</details>


<br/>


## 커밋 히스토리
[커밋 히스토리 확인하기](https://github.com/P-uyoung/X-Corps_Soil-detection/commits/main)

<!-- ## 6. 회고 / 느낀점
>프로젝트 개발 회고 글: https://zuminternet.github.io/ZUM-Pilot-integer/ -->

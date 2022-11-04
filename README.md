# X-Corps_Soil-detection
SNU X-Corps Project : detection of SOC using deep learning 


Logistics

1. Data selection
    1. 기간
        1. 2022년 6월부터 2022년 10월까지 sentinel2 위성 다중분광 이미지
    2. 지역
        1. 대한민국 이천시  
    3. Bare soil
        1. 선행 연구에 의하면 bare soil이어야 sentinel-2A remote sensing data와 SOC와의 관계가 reliable함
        2. NVDI (Normalized difference vegetation index)
            1. NVDI가 낮은 토양은 bare하다고 볼 수 있으므로 NVDI가 낮은 data를 고른다
            2. 0.3 이하
        3. BSI (Bare Soil Index)
            1. BSI가 높은 토양은 bare하다고 볼 수 있으므로 BSI가 높은 data를 고른다.
            2. 0.1 이상
    4. S2A remote sensing data
        1. B2 ~ B12
2. Data type
    1. 10m resolution
    2. 20m resolution
3. Feature
    1. 1단계
        1. B2 ~ B12
        2. NDVI
        3. BSI
    2. 2단계
        1. B2 ~ B12
        2. NDVI
        3. BSI
        4. 토양에서 직접 추출한 feature
            1. SWHC (Soil Water Holing Capacity)
            2. Sand (%)
4. Label
    1. SOC (Soil Organic Carbon)
6. Normalize
    1. Min-Max Normalization
7. Model
    1. 1단계: 20m resolution data를 가지고 다음 네 가지method로 모델링한다.
        1. RF (Random Forest)
        2. SVM (Support Vector Machine)
        3. PLSR (Partial Least Squares Regression)
        4. ANN (Artificial Neural Network)
    2. 2단계
        1. 1단계에서 제일 잘 fit 되는 모델로 다음과 같이 네 번의 모델링을 한다.
        
        <img width="398" alt="image" src="https://user-images.githubusercontent.com/63593428/199702219-f815e88a-d5fa-43b0-b08d-529329d61ace.png">
        
    3. training data set:validation data set =  3:1

    
        	

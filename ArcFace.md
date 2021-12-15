# ArcFace(Additive Angular Margin Loss for Deep Face Recognition) #

### Abstract ###

- ArcFace는 얼굴 인식 분야에서 사용되는 loss

- 대규모 얼굴 인식(Face Recognition)에서 깊은 CNN을 사용하는 feature 학습의 중요한 과제 중 하나는 분별력을 향상시키는 적절한 손실함수 설계이다.

- Centre Loss : 클래스 내 빽빽함(Compactness)를 달성하기 위해 유클리드 공간에서 deep features과 이 feature에 해당하는 클래스 Centres 사이의 거리에 패널티를 부과

- SphereFace : 마지막 완전 연결층(Fully Connected Layer)에서 선형 변환 행렬이 각도 공간(Angular space)의 클래스 Centres 표현으로 사용될 수 있으며 deep features와 이 features에 해당하는 가중치들 사이의 증식(Multiplicative) 방법으로 angle에 패널티를 부ㅜ과

- 최근 유명한 연구는 얼굴 클래스 구분을 최대화하기 위해서 잘 구축된 손실함수에서 margins을 통합하는 것이다.

- 이 논문은 얼굴 인식에 대한 매우 분별력 있는 features를 얻기 위한 Additive Angular Margin Loss(ArcFace)를 제시한다.

- 제시된 ArcFace는 Hypersphere

# 논문 링크 # 
https://arxiv.org/abs/1801.07698

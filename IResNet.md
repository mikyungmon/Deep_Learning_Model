# Improved Residual Networks for Image and Video Recognition # 

### 🔉 Abstract ### 

- ResNet은 다양한 작업에서 널리 채택되고 사용되는 강력한 CNN 아키텍처이다.

- 해당 논문은 ResNet의 개선된 버전을 제안한다.

- 제안하는 개선사항은 ResNet의 세 가지 중요 구성 요소인 네트워크 layer을 통한 flow of information, residual building block, projection shortcut을 모두 다룬다.

- ResNet에 비해 정확도와 학습 수렴에서의 개선을 보여줄 수 있다.

- 제안한 방식을 사용한다면 매우 깊은 네트워크를 훈련할 수 있지만 baseline은 심각한 최적화 문제를 보여준다. 

  📍 *baseline이란 ? 머신러닝 모델이 의미있기 위해 넘어야 하는 최소한의 성능. 이미지 분류(Image Classification)에서는 일반적으로 사람의 성능 (Human-Level Performance: HLP)을 베이스라인 지표로 잡음. 머신러닝 모델이 사람보다 이미지 분류를 잘한다면 의미있는 모델이 구축 됐다고 주장할 수 있을 것임*

- ImageNet 데이터 셋에서 404층의 심층 CNN을 성공적으로 훈련하고 CIFAR-10 및 CIFAR-100에서 3002층 네트워크를 성공적으로 훈련했지만 baseline은 그러한 극한 깊이에서 수렴할 수 없다.

- 해당 논문은 6개의 데이터 셋에 대한 세 가지 작업에 대한 결과를 보고한다.

### 🔉 1. Introduction ### 

- 네트워크의 깊이는 수많은 작업에서 인상적인 결과를 가져오는 강력한 represetation을 얻는 데 핵심 요소 중 하나로 강조되었다.

- 지난 몇 년 동안 CNN의 깊이는 지속적으로 증가해왔다. 그러나 깊이가 증가함에 따라 최적화, 학습의 어려움도 함께 커진다.

- 따라서 더 많은 layer를 추가한다고 해서 더 나은 결과가 보장되는 것은 아니다.

- ResNet은 매우 깊은 CNN 학습 문제를 residual 학습 측면에서 해결책을 제안했다. 실제로 ResNet은 심층 CNN을 학습하는데 매우 강력하며 복잡한 작업의 backbone/foundation이 된다.

- ResNet의 핵심 아이디어는 building block에 대한 identity mapping을 shortcut/skip connection을 사용하여 용이하게 하는 것이다.

- 실제로 옵티마이저가 identity mapping을 하는건 쉽지않고 이것을 degradation 문제(저하 문제)라고 한다. 

- ResNet의 아이디어는 성능 저하 문제를 해결하여 훨씬 더 깊은 네트워크를 효율적으로 학습할 수 있다는 것이다.

- 그러나 degradation 문제가 완전히 해결되지 않았고 해당 논문의 실험에서 증명되었다. 예를 들어 ImageNet데이터 셋에서 깊이를 152개에서 200개로 늘리면 train error를 포함한 훨씬 더 나쁜 결과가 나타나 심각한 최적화 문제가 있음을 시사한다.
    
    ➡ layer수가 증가할 때 ResNet은 여전히 네트워크를 통한 정보의 propagation을 방해한다는 것을 보여준다.
    
- 따라서 해당 논문에서는 정보 propagation을 용이하게 하는 개선된 아키텍처를 제안한다. 네트워크 단계로 분리하고 각 단계 내의 위치에 따라 다른 building block을 적용한다.
  
    ➡ 매우 심층적인 네트워크 학습이 가능하며 깊이가 증가해도 최적화의 어려움은 보이지 않았다.
- 

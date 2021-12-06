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

- ResNet에서는 building block의 차원이 next builidng block의 차원과 맞지 않을 때, projection shortcut을 사용했다. 

- 이 작업은 degradation문제를 위해 필수적이지 않다는 결론을 내렸다. 그러나 projection shortcut은 주요 정보 전파 경로에서 발견되어 신호를 쉽게 동요시키거나 정보 손실을 일으킬 수 있기 때문에 네트워크 아키텍처에서 중요한 역할을 할 수 있다.

- 따라서 여기에서는 매개변수가 없고 성능이 크게 향상되는 개선된 shortcut projection을 소개한다.

- 원래 ResNet에서는 깊이가 상당히 증가할 때 매개변수의 수와 계산 비용을 제어하기 위해 bottleneck building block을 사용했었다. 그러나 이 building block 구조에서 spatial 필터 학습을 담당하는 유일한 컨볼루션은 최소의 input/output 채널을 수신한다.

- 해당 논문은 spatial 컨볼루션에 초점을 변경하는 building block을 제안한다. 원래 ResNet보다 building block에 4배 더 많은 spatial 채널을 포함하는 동시에 매개변수의 수와 계산 비용을 제어한다.

💡 요약하면 해당 논문의 contribution은 다음과 같다.

  1) 단계 기반의 residual learning을 위한 네트워크 아키텍처를 소개한다. 제안된 접근 방식은 정보가 네트워크 layer를 통해 전파되는 더 나은 경로를 제공하여 학습 프로세스를 용이하게 한다.

  2) 정보 손실을 줄이고 더 나은 결과를 제공하는 개선된 projection shortcut을 제안한다.

  3) 보다 강력한 spatial 패턴을 학습하기 위해 spatial 채널을 상당히 증가시키는 building block을 제시한다.

  4) 제안된 접근 방식은 baseline에 대해 일관된 개선을 제공한다.


### 🔉 2. Improved residual network ### 

   #### 2.1 Improved information flow through the network #### 
   
   - ResNet은 많은 ResBlock을 쌓아서 구성된다.
   
   ![image](https://user-images.githubusercontent.com/66320010/144838766-89c645d9-b17f-40ac-a016-606577961563.png)
   
   - 위 그림에서 원래 ResBlock에서 큰 회색 화살표는 정보가 전파되는 가장 직접적인 경로를 나타낸다.

   - 그러나 주 전파 경로(main propagation path)에 ReLU 활성화 함수가 있다. 이 ReLU는 음의 신호를 영점화함으로써 정보 propagation에 부정적인 영향을 미칠 수 있다. 
   
   - 그래서 마지막 BN층과 ReLU를 처음으로 이동하여 pre-activation(사전 활성화)라고 불리는 재설계된 ResBlock을 제안했다(그림 (b)).

   - 하지만 이 두가지 방법 모두 최적이 아니며 다른 문제가 있다. 첫째, 네 단계 모두에 걸쳐 전체 신호의 정규화(BN)가 없으므로 더 많은 블록을 추가함에 따라 전체 신호가 더 "비정규화"되어 학습에 어려움이 발생한다. 이 문제는 원래의 ResNet과 pre-act모두에 존재한다.

   - 둘째, 4개의 prejection shortcut이 있다는 점에 유의하자. 이론적으로 네트워크는 대부분의 block에 대한 identity matching을 학습할 수 있다. 0을 출력할 수 있으므로 더하기 연산을 적용할 때 identity matching을 쉽게 생성할 수 있다.
   
   - 이 경우 모든 4개의 주요 단계에 대한 pre-activation, ResNet은 4개의 연속적인 1 x 1 conv로 끝나지만 그 사이에 비선형성이 없어 학습 능력이 제한된다.

   - 해당 논문은 각 메인 스테이지 이전에 신호를 안정화하고(각 메인 스테이지 이후에 전체 신호에 BN을 사용함) 적어도 하나의 비선형성 (전체 신호에도 적용됨) 이 있음을 보장하기 때문에 이 두가지 문제도 해결한다.

   - 제안한 ResBlock은 위 그림(c)와 같다. 구체적인 예로서 다음 표에 나와있는 50 깊이의 ResNet(ResNet-50)을 취할 수 있고 이거는 어느 깊이로도 확장될 수 있다.
    
   ![image](https://user-images.githubusercontent.com/66320010/144841621-2ddef929-7bf8-4cd7-b815-14b9b13eb5e4.png)
   
   - ResNet-50의 가능한 단계 분리는 출력 spatial 크기와 출력 채널 수에 따라 결정된다.

   - 출력 spatial 크기나 출력 채널 수가 변경될 때 다른 단계의 시작을 표시한다. ResNet-50의 경우 4개의 주요 단계와 시작 및 종료 단계를 얻는다. 4개의 주요 단계 각각은 다수의 ResBlock을 포함할 수 있다.

   - ResNet-50의 경우 1단계에 3개의 ResBlock, 2단계에 4개, 3단계에 6개, 4단계에 3개가 있다.
  
   - 각 주요 단계는 세 부분으로 나뉜다. 하나는 Start ResBlock, 여러 개의 Middle ResBlock, 하나는 End ResBlock이다.

   - 여기서 ResNet을 제안된 단계 아키텍처로 분할한 결과를 ResStage 네트워크라고 부른다.

   #### 2.2 Improved projection shortcut #### 
   #### 2.3 Grouped building block #### 
   

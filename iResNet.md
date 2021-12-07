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
   
   - 제안된 ResStage의 경우 Start ResBlock에 의해 신호가 이미 정규화되었기 때문에 각 주요 단계에서 첫 번째 middle ResBlock에서 첫 번째 BN을 제거한다. 
   
   - 제안된 ResStage는 워드와 백워드에 대한 정보가 전파되는 주요 경로에 고정된 수의 ReLU를 포함한다. 예를 들어 주 전파 경로에 있는 ReLU의 수는 네트워크 깊이에 정비례한다.
   
   - ResStage에 있는 동안 주요 단계의 경우 기본 정보 전파 경로에 ReLU가 4개뿐이며 깊이 변경의 영향을 받지 않는다. 이를 통해 정보가 여러 계층을 통과할 때 네트워크가 신호를 방해하는 것을 방지할 수 있다.

   - 각 단계의 End Resblock은 BN과 ReLU로 완료되며, 이는 다음 단계를 위한 준비, 안정화 및 새로운 단계로 진입하기 위한 신호를 준비하는 것으로 볼 수 있다.

   - Start ResBlock에는 신호를 정규화하고 projection shortcut(정규화된 신호도 제공)를 사용하여 요소별 추가를 위해 준비하는 마지막 conv 뒤에 BN 층이 있다.

   - 제안한 접근 방식은 정보가 네트워크를 통해 전파되는 더 나은 경로를 제공함으로써 학습을 용이하게 한다. 
  
   - ResStage는 최적화를 용이하게 하여 매우 깊은 네트워크를 쉽게 훈련할 수 있도록 한다. 
  
   - 네트워크는 학습 과정에서 어떤 ResBlock을 사용하고 어떤 ResBlock을 버릴지(가중치를 쉽게 0으로 설정하여) 쉽게 동적으로 선택할 수 있다. 단계별 학습을 위한 제안은 효율적인 정보 흐름을 위해 설계되었지만 신호를 제어할 수 있도록 설계되었다.

   #### 2.2 Improved projection shortcut #### 
   
   - 원래 ResNet 아키텍처에서 x의 차원이 F의 차원 출력과 일치하지 않을 때 요소별 추가를 가능하게 하기 위해 identity shortcut대신 projection shortcut이 x에 적용된다.

   - 원래 ResNet에서 사용된 기본 projection shortcut은 다음 그림에서 (a)와 같다.

   ![image](https://user-images.githubusercontent.com/66320010/144979821-7629871f-64c4-4d92-83bc-a9b97061d6a0.png)

   - 원래 projection shortcut은 1 x 1 커널의 컨볼루션을 사용하여 x의 채널을 F의 출력 채널 수로 projection한다(1 x 1 컨볼루션은 stride 2로 진행하고 F의 출력과 요소별 덧셈 전에 BN이 적용된다).
   - 그러므로 채널과 spatial matching은 1 x 1 컨볼루션에 의해 수행된다.
   
   - 이는 spatial size를 2로 줄일 때 1 x 1 컨볼루션(with stride 2)이 featrue maps activations의 75%를 건너뛰기 때문에 정보에 상당한 손실을 초래한다. 또한 1 x 1 컨볼루션에서 고려되는 feature map activation의 25%를 선택하기 위한 의미 있는 기준이 없다.
   
   - 그 후 결과는 ResBlock의 output에 더해진다. 따라서 projection shortcut의 noisy output은 다음 ResBlock에 상대적으로 정보의 절반을 기여한다.
   
   - 이것은 noise와 정보 손실을 야기하고 네트워크를 통한 정보의 주요 흐름을 부정적으로 교란시킬 수 있다.

   - 제안된 projection shortcut은 위의 그림에서 (b)와 같다. 채널 projection에서 채널 projection을 분리한다.
   - 그리고 spatial projection을 위해 stride 2로 3 x 3 max pooling을 수행한다.
   - 그런다음 채널 projection을 위해 stride 1로 1 x 1 컨볼루션을 적용한 다음 BN을 적용한다.
   - max pooling과 함께, 활성화를 위해 1 x 1 컨볼루션대해 고려되어야하는 criterion을 도입한다.
   - 또한 spatial projection은 feature map의 모든 정보를 고려하고 다음 단계에서 고려할 가장 높은 활성화를 가진 요소를 선택한다.
   - max pooling 의 커널은 ResBlock의 middle conv의 커널과 일치하므로 동일한 spatial window에서 계산된 요소간의 추가적인 요소가 수행된다.
   - 제안한 projection shortcut은 정보 손실을 줄이고 실험에서 성능상의 이점을 보여준다.
   - 정보 손실과 신호 동요를 줄이기 위한 첫번째 motivation 이외에도 제안한 projection shortcut이 다른 두 가지 이유가 있다.
   - 두 번째로, 각 주요 단게의 start ResBlock에 max pooling을 갖는 것은 네트워크 변환 불변성을 개선하고 궁극적으로 전반적인 인식 성능을 향상 시킨다.
   - 세 번째로는 다운 샘플링을 수행하는 각 단계의 start ResBlock은 3 x 3 컨볼루션에 의해 수행되는 소프트 다운 샘플링 간의 조합으로 볼 수 있다.
   - 하드 다운샘플링은 분류(가장 높은 활성화가 있는 요소)에 유리하고 소프트 다운 샘플링은 모든 spatial 맥락을 잃지 않는데 기여한다(따라서 요소간의 전환이 더 원활할 때 더 나은 localization을 돕는다)
   - 제안된 projection shortcut은 모델에 추가 매개변수를 추가하지 않는다. ResNet은 일반적으로 각 단계의 시작(단계의 ResBlock 시작)에 4개의 projection shortcut만 필요합니다. 
   - 따라서 제안된 projection shortcut의 경우 일반적으로 계산 비용이 저렴한 최대 풀링 레이어 3개만 추가로 포함하면 되기 때문에 ResNet의 계산 비용 증가는 무시할 수 있다.
   - 제안된 projection shortcut와 함께 이전 섹션의 단계 아이디어를 사용할 때 개선된 잔여 네트워크(iResNet)를 참조한다.
  
   #### 2.3 Grouped building block #### 
   
   - bottleneck building block은 네트워크 깊이를 증가시킬 때 합리적인 계산 비용을 유지하기 위한 실질적인 고려사항으로 도입되었다.
   
   - 먼저 채널 수를 줄이기 위한 1 x 1 컨볼루션, 가장 작은 수의 입력/출력에서 작동하는 3 x 3 컨볼루션 bottleneck, 마지막으로 원래의 채널 수로 다시 늘리는 1 x 1 컨볼루션을 포함한다.
   
   - 이 설계의 이유는 적은 수의 채널에서 3 × 3 컨볼루션을 실행하여 계산 비용과 매개 변수의 수를 제어하기 위함이다.
   - 그러나 3×3 conv는 공간 패턴을 학습할 수 있는 유일한 구성 요소이므로 매우 중요하지만 bottleneck 설계에서는 더 적은 수의 입력/출력 채널을 수신한다.
   - 해당 논문은 3×3 컨볼루션에서 가장 많은 수의 입력/출력 채널을 포함하는 개선된 building block을 제안한다.
   - 제안된 building block의 설계에서 그룹화된 컨볼루션을 사용하며 이를 ResGroup Block이라고 한다.
   - 그룹화된 컨볼루션은 계산 비용과 메모리로 인한 제한을 극복하기 위해 두 개의 GPU에 모델을 배포하는 솔루션으로 사용되었다.
   - 그룹화된 컨볼루션의 주요 아이디어는 입력 채널을 여러 그룹으로 분할하고 각 그룹에 대해 독립적으로 컨볼루션 작업을 수행하는 것이다.
   - 이러한 방식으로 매개변수(param)와 부동소수점 연산(FLOP)의 수를 그룹 수와 동일한 비율로 줄일 수 있다.

   ![image](https://user-images.githubusercontent.com/66320010/144991624-67567e4f-a59a-489c-a753-752d9bd63e1f.png)
   
   - 여기서 chin과 chout은 입력 및 출력 채널의 수, k1과 k2는 컨볼루션의 kernel의 크기를 나타내고 w와 h는 채널의 너비와 높이, G는 채널이 분할된 그룹의 수를 나타낸다.
   
   - 기존 ResNet-50과 유사한 계산 비용과 매개변수 수를 갖는 제안된 네트워크 아키텍처는 위에서 언급한 표에 나와 있다.
   
   - 매개변수의 수와 계산 비용을 제어하기 위해 그룹화된 컨볼루션은 3 × 3 spatial 커널과 함께 사용된다.
 
   - 두 가지 아키텍처를 제안
      1. ResGroupFix-50은 각 단계에 대한 그룹 수가 고정된 경우를 나타냄. 이 옵션은 50개 레이어에 대한 baseline보다 유사한 수의 FLOP와 8.57% 적은 파라미터를 생성함.
      
      2. ResGroup-50은 모든 스테이지가 그룹당 동일한 수의 채널을 갖는 방식으로 채널에 그룹 수를 조정하는 경우를 나타냄. ResGroup-50은 원래 ResNet-50 및 더 많은 FLOP와 유사한 수의 매개 변수를 가지고 있음.
   
   ![image](https://user-images.githubusercontent.com/66320010/144993808-31f0ac9f-48e6-4d01-bf79-a3aeb0762cbd.png)
   
   - 네트워크의 마지막 잔여 빌드 블록에 대한 예시적인 비교는 위 그림을 참조
   
   - 이 접근법으로, 3×3은 가장 많은 수의 채널을 가지고 있고 spatial 패턴을 학습할 수 있는 더 높은 능력을 가지고 있다.
   

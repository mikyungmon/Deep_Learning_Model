# MTCNN # 

- MTCNN은 CNN을 활용하여 얼굴 검출 분야에서 정확도와 성능을 끌어올린 모델이다.

- 얼굴 검출 분야에서 높은 성능과 빠른 속도를 보여주어 많은 논문과 프로젝트에서 사용되고 있다.

- 💡 영향력 - CNN을 이용하여 얼굴 검출 정확도를 95% 수준까지 끌어올렸으며 약 1300회 인용됨

- 💡 주요 기여 - face detection, bounding box regression, face alignment 세 가지 테스크를 동시에 학습시키는 joint learning 방식을 제안하였으며 이를 통해 더 빠른 속도와 높은 정확도를 달성함

- 해당 논문은 얼굴을 검출해내는 face detection과 눈,코,입 좌표를 알아내는 face alignment 두 가지 테스크가 서로 긴밀하게 연결되어 있을 것이라는 아이디어에서 출발한다.

- 여기에 얼굴 위치를 나타내는 박스의 위치를 세밀하게 조절해주는 bounding box regression 테스크를 추가하여 세 가지 테스크를 동시에 학습시키면 시너지가 날 것이라며 학습 자체도 단순해질 것이라고 주장한다.

- 저자들은 이러한 새 테스크를 학습한 P-net, R-net, O-net이라는 세 CNN을 차례로 통과하는 Cascade 모델을 제안한다.

### 🔉 Architecture ### 

MTCNN은 3개의 neural network(P-net, R-net, O-net)로 이루어져 있다.

이 때 각 net에서 face classification, bbox regression, face landmark localization 과정을 진행하면서 동시에 학습시키는 방식(joint learning)을 사용한다.

joint learning을 통해 기존의 다른 방법에 비해 정확도가 높고 속도가 빨라 지금까지 많이 쓰이고 있는 논문이다.

전반적인 구조는 아래 그림과 같으며 단계적으로 이러한 네트워크가 어떻게 구성되었는지 알아본다.

![image](https://user-images.githubusercontent.com/66320010/145935114-f44003b1-cdf7-4a4e-aa76-aa5842a7372c.png)

우선 본격적으로 detection이 이루어지기 전에 다음과 같이 input되는 이미지를 각기 다른 scale로 resize하여 image pyramid를 만든다.

![image](https://user-images.githubusercontent.com/66320010/145935616-f34db365-d15c-4e48-acd5-00060d4ecfa7.png)

다양한 scale의 이미지로 다양한 사이즈의 얼굴을 더 잘 detection하기 위해서라고 볼 수 있다.

### 1. P-net(Proposal Network) ###

![image](https://user-images.githubusercontent.com/66320010/145936048-f44cef71-a074-4b00-b0ed-29ddd36cc6cb.png)

- P-net은 12 * 12 * 3 크기의 작은 이미지를 입력으로 받는다. 

- 컨볼루션만 거쳐서(Fully Connected Layer 없음) 해당 영역이 얼굴인지 아닌지 나타내는 face classificatioin, 얼굴 영역을 나타내는 좌측 상단 꼭지점의 x,y좌표와 박스의 너비,크기를 나타내는 4개의 bounding box regression값, 그리고 양쪽 눈,코, 양쪽 입꼬리의 x,y좌표를 나타내는 10개의 landmark localization값을 결과로 리턴한다.

- 앞서 만든 이미지 피라미드를 입력받아서 각각의 이미지에 12 * 12 크기의 윈도우로 스캔하며 얼굴에 해당하는 영역을 찾아내는 것이다.

- 찾아낸 얼굴 영역은 다시 원래의 사이즈로 resize한다. 예시를 들어보면 30x20 크기 이미지에서 찾은 얼굴 영역 좌표를 300x200 이미지에 해당하는 좌표로 변환해주는 것이다.

- 이렇게 찾은 박스들을 대상으로 Non-Maximum-Suppression(NMS)과 bounding box regression(BBR)을 적용해주어 높은 정확도의 후보 영역만 남도록 추려낸다.

### 2. R-net(Refine Network) ###

![image](https://user-images.githubusercontent.com/66320010/145938851-9853f124-3850-4bc5-b6fb-d40ba2c4cf7d.png)

P-net을 통해서 얼굴로 추정되는 박스들의 리스트를 얻었다. 

- R-net의 역할은 이 박스들 중에서도 진짜 얼굴에 해당하는 영역들을 추려내고 bounding box regression을 더 정교하게 수행하는 것이다.

- P-net과의 차이점은 뒤에 FC layer가 추가된다는 점이다. 또한 앞에서 구한 박스들에 대해 input size를 24 * 24로 resize를 진행하는 작업이 필요하다는 것이다.

- R-net의 output역시 P-net과 같이 Non-Maximum-Suppression(NMS)과 bounding box regression(BBR)을 적용하여 가장 확률이 높은 박스들에 대한 output값들을 남긴다.

### 3. O-net(Output Network) ###

![image](https://user-images.githubusercontent.com/66320010/145939459-83a82238-3d31-451c-85bd-22f78e3dbb8d.png)

- O-net은 R-net을 통해 찾아낸 박스들을 모두 48 * 48 크기로 resize한 것을 입력으로 받는다(필터의 크기를 키우면서 얼굴에 해당하는 더 추상적인 정보를 찾아내기 위한 의도로 보임).

- 모델의 깊이가 조금 더 깊어졌다. 여러 Conv 레이어와 FC레이어를 거친 뒤 세 종류의 output을 내게 되며 이것이 최종 face classification, bbox regression, face landmark localization 결과 값이 된다.

각 단계에서 얻어내는 값들을 자세히 살펴보면 다음과 같다.

- face classification (2개) :
ydet = GT에서 얼굴이 있는지 여부(있을때 1, 없을때 0),
p = 얼굴이 있을 확률
- bbox regression (4개) : 
예측한 bbox의 왼쪽상단 x,y좌표,
예측한 bbox의 너비와 높이
- face landmark localization (10개) :
왼쪽 눈의 x,y 좌표, 
오른쪽 눈의 x,y 좌표, 
코의 x,y 좌표, 
입의 왼쪽 끝 부분의 x,y 좌표, 
입의 오른쪽 끝 부분의 x,y 좌표 

### 🔉 Loss function ### 

MTCNN은 classification, regression, localization 세 가지 테스크에 대하여 각각 loss를 구한 뒤 이를 가중치를 두어 합치는 방법을 취한다.

#### Face classification loss #### 

![image](https://user-images.githubusercontent.com/66320010/145940802-b87ca588-3ac0-4bc7-9c69-932f194b4713.png)

- ydet(GT에서 얼굴이 있는지 없는지 여부 (있을 때 1, 없을 때 0)) 와 p(얼굴이 있을 확률)의 cross entropy 값을 통해 face classification loss를 구한다.

#### Bounding box regression loss #### 

![image](https://user-images.githubusercontent.com/66320010/145941160-25cbc872-948c-42e7-887f-4ed10e2a0b3d.png)

- bbox의 왼쪽상단 x,y좌표, 높이, 너비로 이루어진 4개의 값과 GT의 값 차이를 유클리드 거리를 측정하여 loss로 사용한다.

- || || 기호는 선형 대수에서의 norm을 나타내며 우측 하단의 2는 각 차원별 차이의 제곱의 합을 구하란 의미이며, 우측 상단의 2는 전체 결과값을 제곱하라는 의미이다. 이를 수식으로 풀어내면 아래와 같다.

![image](https://user-images.githubusercontent.com/66320010/145942017-51f2d8dd-6ba1-4658-9609-2bb050adecdc.png)

#### Facial landmark Localization loss #### 

![image](https://user-images.githubusercontent.com/66320010/145942108-1bb162c2-384e-4ad5-a69f-d6b0250346e5.png)

- 얼굴의 다섯개의 포인트(두 눈, 코, 입의 양쪽 끝 부분)의 x,y좌표 값과 GT의 값의 차이를 유클리드 거리로 구한다.

- 10차원 벡터로 볼 수 있으며 regression loss와 비슷하게 loss계산한다.

#### Total loss ####

![image](https://user-images.githubusercontent.com/66320010/145942461-b7830dee-3cff-4e89-9d2c-829d22894bac.png)

세 가지 loss를 엮은 최종 loss이다.

- 최종 loss는 각각의 loss에 가중치를 반영하여 모두 합쳐준 것이며 수식과 P,R,O net별 가중치는 아래와 같다. 

![image](https://user-images.githubusercontent.com/66320010/145942713-f36547c9-1057-4cf4-87c4-b5af6ebdd6bc.png)

- N은 전체 데이터 수이며 베타는 특정 샘플이 선택되었는지 아닌지를 0과 1로 표현한다. 

- 즉, Stochastic gradient descent를 적용하여 전체 데이터 셋에서 일부만 뽑아내어 loss를 계산한다는 의미이다.

- αj는 각 단계마다 각 loss의 가중치를 다르게 주기위해 사용되었다. 이 논문에서는 P-Net과 R-Net에 (αdet = 1, αbox = 0.5, αlandmark = 0.5)를 주어 detection에 중점을 두었다.

- Lj은 위에서 구한 각 목적별 loss값이다.

![image](https://user-images.githubusercontent.com/66320010/145943073-195e573e-5fb7-4451-9442-b225d6043666.png)

MTCNN은 위와 같은 loss값으로 joint training을 진행하는데 위의 그래프에서 보면 알 수 있듯이 joint training을 통해 큰 폭으로 성능이 높아진 것을 알 수 있다.

### 🔉 model 학습 및 성능 ### 

논문에서는 전체 loss와 모델 구조만 제시해주고 구체적으로 어떻게 학습 시켰는지는 언급하지 않는다. 

아래는 기존 논문과의 단계별 성능 비교이다.

![image](https://user-images.githubusercontent.com/66320010/145943273-473a45d0-a812-41e0-bc40-588dbdf54638.png)

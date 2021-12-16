# FaceNet #

- 2015년에 처음 나온 논문으로 얼굴 인식 모델에서 가장 중요한 개념인 triplet loss가 처음 등장된 논문이며 7000번이 넘게 인용되었다.

- CNN feature를 L2 norm + triplet loss를 사용하는 새로운 face recognition 방법론을 제안한다.

- 이를 학습시키기 위해서는 hard triplet seleciton이 중요함을 강조한다.

## Abstract ## 

- 얼굴 인식 분야는 많은 발전이 있었음에도 불구하고 대규모로 얼굴 확인 및 인식을 효율적으로 구현하는 것은 현재 접근 방식에 문제를 제시한다.

- 해당 논문에서는 얼굴 이미지로부터 거리가 얼굴 유사성의 척도에 직접적으로 대응하는 컴팩트한 유클리드 공간으로의 매핑을 직접 학습하는 FaceNet이라는 시스템을 제안한다.

- 이 공간이 생성되면 FaceNet 임베딩을 특징 벡터로 사용하는 표준 기술을 사용하여 얼굴 인식, 검증 및 클러스터링과 같은 작업을 쉽게 구현할 수 있다.

- 이 방법은 기존 접근 방법에서와 같이 중간 Bottleneck Layer가 아니라 임베딩 자체를 직접 최적화하도록 훈련된 딥 컨볼루션 네트워크를 사용한다.

- train을 위해 새로운 온라인 triplet mining 방법을 사용하여 생성된 대략적 정렬된 매칭/비매칭 얼굴 패치의 triplet을 사용한다.

- 해당 접근 방식의 이점은 뛰어난 representational efficiency이다. 얼굴당 128 바이트만 사용하여 최첨단 얼굴 인식을 달성한다.

- labeled Faces in the Wild(LFW) 데이터 셋에서 99.63%의 정확도를 달성했으며 YouTube Faces DB에서는 95.12%를 달성했다.

## Introduction ## 

- 이 논문에서는 Face-Verification(동일인물인지), Recognition(누구인지), Clustering(공통되는 사람들 찾기) 세가지의 통합 시스템을 소개한다.

- 이 방법은 이미지마다 Deep Conv Net을 사용하여 이미지당 유클리드 임베딩을 학습하는 것을 기반으로 한다.

- 제곱된 L2 거리가 얼굴 유사성에 직접 대응하도록 훈련되었다(유클리드 거리의 제곱은 얼굴 유사도와 곧바로 일치한다). 즉 같은 사람의 얼굴은 적은 거리를 가지고 다른 사람일 경우 큰 거리를 가지게 된다.

![image](https://user-images.githubusercontent.com/66320010/146324530-a1892169-4c8a-4439-92b1-721a9650a552.png)

- FaceNet은 얼굴 사진에서 그 사람에 대한 특징 값(=feature, embedding결과)를 구해주는 모델로 이 값을 활용하여 값들간의 거리를 통해 이미지에 대한 identification, clustering을 할 수 있게 한다.

- 이 임베딩이 생성되면 앞서 언급한 작업들이 간단해진다. 인식은 k-NN 분류 문제가 되고 클러스터링은 k-mean 또는 응집 클러스터링과 같은 기존 기술을 사용하여 달성할 수 있다.

- 딥 네트워크를 기반으로 하는 이전의 얼굴 인식 접근 방식은 알려진 얼굴 ID 세트에 대해 훈련된 분류 계층을 사용한 다음에 사용된 ID세트를 넘어 인식을 일반화하는 방법으로는 중간 bottleneck layer를 사용한다.

- 이 접근 방식의 단점은 간접성과 비효율성이다. bottleneck representation이 새로운 얼굴에 잘 일반화되기를 바라야한다.

- bottlneck layer를 사용하면 얼굴 당 표현의 크기가 매우 커진다(1000차원). 일부 최근 연구에서는 PCA를 사용하여 이 차원을 감소시켰지만 이것은 네트워크의 한 계층에서 쉽게 학습할 수 있는 선형 변환이다.

- 이러한 접근 방식과 대조적으로 FaceNet은 LMNN을 기반으로 하는 triplet 기반 손실함수를 사용하여 출력을 128차원 임베딩으로 직접 훈련한다.

- triplet은 2개의 일치하는 얼굴과 하나의 다른 얼굴로 이루어져 있으며 loss는 distance margin으로 일치하는 쪽을 불일치하는 쪽으로부터 분리해내는 데 초점을 두고 있다.

- 썸네일(thumbnails)은 얼굴 영역의 딱 맞게 자르기이며 배율 및 변환이 수행되는 것 외에는 2D 또는 3D 정렬이 수행되지 않는다.

- 사용할 triplet을 선택하는 것은 좋은 성과를 달성하는데 매우 중요하며 커리큘럼 학습에서 영감을 받아 네트워크가 훈련됨에 따라 triplet의 난이도를 지속적으로 증가시키는 새로운 온라인 negative exemplar mining 전략을 제시한다.

- 클러스터링 정확도를 개선하기 위해서는 한 사람의 임베딩을 위한 구형 클러스터를 권장하는 hard positive mining 기술도 탐색한다(hard positive : 같은 사람인데 다르게 보이는 사람).

## Related work ## 

- 해당 논문의 접근 방식은 얼굴의 픽셀에서 직접 표현을 학습하는 순수 데이터 기반 방법이다. 

- 엔지니어링된 기능을 사용하는 대신 지정된 얼굴의 대규모 데이터 셋을 사용하여 포즈, 조명 및 기타 변형 조건에 대한 적절한 불변성을 얻는다.

- 이 논문에서는 최근 컴퓨터 비전 커뮤니티에서 큰 성공을 거둔 두 가지 다른 심층 네트워크 아키텍처를 탐구하며 둘 다 딥 컨볼루션 네트워크이다.

- 첫 번째 아키텍처는 Zeiler&Fergus Model에 기반을 둔 다중 interleave된 컨볼루션 레이어, 비선형 활성화, 로컬 reponse 정규화 및 max pooling layer로 구성된 모델을 기반으로 한다. 여기에 1 * 1 * d 컨볼루션 레이어를 추가하여 사용한다.

- 두 번째 아키텍처는 Inception 모델을 기반으로 한다. 

이러한 네트워크는 여러 다른 컨볼루션 및 pooling layer를 병렬로 실행하고 response를 연결하는 혼합 레이어를 사용한다.

이러한 모델이 매개변수 수를 최대 20배까지 줄일 수 있고 유사한 성능에 필요한 FLOPS 수를 줄일 수 있다는 것을 발견하였다. 

(FLOPS : 플롭스(FLOPS, FLoating point Operations Per Second)는 컴퓨터의 성능을 수치로 나타낼 때 주로 사용되는 단위이다. 초당 부동소수점 연산이라는 의미로 컴퓨터가 1초동안 수행할 수 있는 부동소수점 연산의 횟수를 기준으로 삼는다)

## Method ## 

FaceNet은 심층 컨볼루션 네트워크를 사용한다. 

Zeiler&Fergus 스타일 네트워크와 최근 Inception 유형 네트워크의 두 가지 아키텍처에 대해 논의한다.

해당 논문의 접근 방식의 가장 중요한 부분은 전체 시스템의 종단 간 학습에 있다. 

이를 위해 얼굴 확인, 인식 및 클러스터링에서 달성하고자 하는 것을 직접 반영하는 triplet loss를 사용한다. 

즉, 우리는 feature space속의 이미지 x로부터 embedding f(x)를 얻어서 같은 사람이면 sqaured distance가 작게, 다른 사람이면 크게 한다.

비록 다른 loss와 직접적인 비교는 하지 않았지만 손실이 하나의 ID의 모든 얼굴이 임베딩 공간의 단일 지점에 투영되도록 권장하기 때문에 triplet loss가 얼굴검증에 더 적합하다고 생각한다는게 저자들의 의견이다.

그러나 triplet loss는 한 사람과 다른 모든 얼굴로부터 나온 모든 얼굴쌍에 대해 margin을 적용하려고 한다. 이를 통해 하나의 ID에 대한 얼굴이 다양하게 존재하는 동시에 다른 ID에 대한 거리 및 식별 가능성을 계속 적용할 수 있다.

![image](https://user-images.githubusercontent.com/66320010/146352390-1659b4c2-611a-4745-b5cd-0dc52eaee247.png)

### 1. Triplet Loss ### 

![image](https://user-images.githubusercontent.com/66320010/146352540-7c16305a-cd60-45a5-9436-9e91cae2d1b0.png)

- Triplet loss는 anchor sample(특정 인물의 이미지), positive sample(anchor 이미지와 닮은 모습의 이미지), negative sample(anchor이미지와 닮지 않은 모습의 이미지), 3개의 샘플에 대해 loss 계산을 수행한다.

- 임베딩은 f(x) ∈ R^d로 나타내어지며 이것은 이미지 x를 d차원의 유클리드 공간에 임베딩시킨다. 이 임베딩의 결과가 거리가 1인 d차원 hypersphere에 존재하도록 한다.

- 여기서 특정 사람의 anchor 이미지가 다른 동일한 사람의 모든 이미지(positive)가 다른 사람의 이미지(negative)보다 가깝다는 것을 보장하고자 한다.

- 즉 같은 사진은 가까운 거리에, 다른 사진 먼 거리에 있도록 유사도를 벡터 사이의 거리와 같아지게 하려는 목적이다.

- 이를 식으로 표현하면 다음과 같다.

![image](https://user-images.githubusercontent.com/66320010/146356839-be68c86b-a8ad-4ce8-a39d-060ae1088481.png)

- 부등호 왼쪽이 anchor과 positive 사이의 거리이고 부등호 오른쪽이 anchor과 negative 사이의 거리이다. α는 positive와 negative 사이에 주고 싶은 margin을 의미한다고 생각하면 된다.

- T는 training set에 있는 모든 가능한 triplets 집합이고 cardinality N을 가진다.

- triplet loss L을 수학적으로 계산하면 아래와 같은 공식이 나온다.

![image](https://user-images.githubusercontent.com/66320010/146357752-d9b3fd85-cc3f-4d26-ba00-6f987d277d18.png)

- 가능한 모든 triplet 들을 생성한다면 쉽게 충족되는 많은 triplet이 생성된다. 이러한 triplet은 훈련에 기여하지 않으며 네트워크를 계속 통과하므로 수렴 속도가 느려진다.

- 활성화되어 모델 개선에 기여할 수 있는 hard triplet을 선택하는 것이 중요하다.

### 2.Triplet Selection ###

- 위의 식을 만족하는 triplet을 만들면서 학습을 진행할 때, 완전 다른 사진일 때는 너무 쉽게 만족하는 경우가 있을 것이다.

- 이런 경우 학습이 제대로 되지않는 문제가 발생한다.

- 따라서 잘 구분하지 못하는 이 식을 만족하지 않는 triplet을 만들어야한다.

- 즉, 빠른 수렴을 위해 위에서 제시한 triplet의 제약을 위반하는 triplet을 선택하는 것이 중요하다.

- 따라서 최대한 먼 거리에 있는 positive를 고르고 최대한 가까운 거리에 있는 negative를 골라야한다.

![image](https://user-images.githubusercontent.com/66320010/146361607-9d05c5b7-9cce-42ba-af44-2bca3ffae836.png)

- 이를 각각 hard positive, hard negative로 표현한다. 

- 하지만 전체 데이터에서 각각 hard point들을 찾아야한다고 할 때 계산량이 많아져 시간이 많이 필요하며 비효율적이고 overfitting이 생길 수 있다. 

- 따라서 이것을 해결하기 위해서 이 논문에서는 mini batch안에 hard point를 찾도록 하는 방법을 제시한다.

- 이때 hard positive를 뽑는 것보다는 모든 anchor-positive쌍을 학습에 사용하는게 실제로 더 안정적이고 약간 더 빠르게 수렴한다.

- hard negative를 뽑을 때는 다음과 같은 식을 만족하는 x중에 뽑는 것이 좋은 성능을 보였다.

![image](https://user-images.githubusercontent.com/66320010/146362145-4f3abbb5-155f-4b90-a534-612a3dee0aac.png)

- 이러한 negative examplars를 semi-hard라고 부른다.

- 이는 anchor-positive보다 anchor-negative간 거리가 더 멀긴 가지만 margin이 충분히 크지 않아서 어렵다.

- 이 negatives는 내부의 margin α에 의존한다. 전에 언급했듯이, 올바른 삼중항 선택은 빠른 수렴을 위해 중요하다. 

- 해당 논문은 확률적 경사하강법(SGD)동안 수렴을 향상시키는 경향이 있는 작은 미니 배치를 사용하고자 한다.

- 먼저 small mini-batch를 사용할 것이고 이것은 SGD(확률적 경사하강법)를 통한 수렴 속도를 향상시킬 것이다. 또한 실행 details는 10에서 수백개의 exemplars들로 이루어진 batch를 더 효율적으로 만들 것이다.

- batch size에 대한 주요 제약은 논문에서 mini-batch로부터 관련성이 높은 triplets를 고르는 방법이다. 

- 논문의 대부분의 실험에서는 약 1800exemplar정도의 batch size를 사용했다.

### 3. Deep Convolutional Networks ### 

- 해당 논문에서는 모든 실험에서 표준 backprop 및 AdaGrad과 함께 SGD를 사용하여 CNN을 훈련시켰다. 

- 대부분의 실험에서 learning rate는 0.05였고 모델을 마무리짓기 위해 더 낮추기도 하였습니다. 

- 모델은 처음에 랜덤으로 초기화되었으며 cpu cluster에서 1000~2000시간 정도 훈련하다. loss의 감소는 500시간이후부터 급격하게 줄어들었지만 추가적인 훈련은 성능을 크게 향상시킬 수 있었다. margin α는 0.2로 설정되었다.

- 논문에서는 두 가지 유형의 아키텍처를 사용했으며 실험 섹션에서 이들의 장단점을 더 자세히 살펴보았다. 실제적인 차이점은 매개변수와 FLOPS의 차이에 있다.

- 다음 표에 표시된 첫 번째 범주는 제안한 대로 Zeiler&Fergus 아키텍처의 표준 컨볼루션 계층 사이에 1x1xd 컨볼루션 계층을 추가하여 22계층 깊이의 모델을 생성한다.

![image](https://user-images.githubusercontent.com/66320010/146369703-e6e2b0fc-1aa7-415a-9c54-d2887669bde4.png)

- 사용하는 두 번째 범주는 GoogLeNet 스타일의 Inception 모델을 기반으로 한다. 이 모델은 매개변수가 20배 더 적고(약 6.6M-7.5M) FLOPS가 최대 5배 더 적다(500M-1.6B 사이).

## Experiments ## 


## Summary ## 

- 얼굴 인증을 위해 유클리드 공간에 임베딩을 직접 학습하는 방법을 제공한다.

- 이것은 CNN bottleneck을 사용하거나 SVM 분류뿐만아니라 여러 모델과 PCA의 연결과 같은 추가 후처리가 필요한 다른 방법과 차별화된다.

- 종단 간 train(end to end)은 설정을 단순화하고 당면한 작업과 관련된 손실을 직접 최적화하면 성능이 향상된다는 것을 보여준다.

- 또 다른 장점은 최소한의 정렬만 필요하다는 것이다(얼굴 영역 주변의 촘촘한 자르기).

- 또한 유사성 변환 정렬을 실험했고 이것이 실제 성능을 약간 향상시킬 수 있음을 확인했다.

- 향후에는 오류 사례를 더 잘 이해하고 모델을 개선하며 모델 크기를 줄이고 CPU요구사항을 줄이는 데 중점을 둘 것이다.

- 그리고 현재 매우 긴 교육 시간을 개선하는 방법, 예를 들어 더 작은 배치 크기와 오프라인 및 온라인 포지티브 및 네거티브 마이닝을 통한 커리큘럼 학습의 변형을 조사할 것이다.


## 논문 링크 ## 

https://arxiv.org/pdf/1503.03832.pdf







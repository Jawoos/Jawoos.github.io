# DeepHunter: Hunting Deep Neural Network Defects via Coverage-Guided Fuzzing

태그: Fuzzing, for AI
읽은 날짜: 2021년 6월 21일
논문 파일: DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/(18_ACM_ASE)_DeepGauge__Multi-Granularity_Testing_Criteria_for_Deep_Learning_Systems.pdf

해당 논문에서는 Coverage Guided Fuzz를 DNN 기반 프로그램에 적용 했을 때의 유용성을 확인하고자 실험을 진행 하였다.

논문에서는 4가지의 질문을 던지고 그에 대한 해답을 찾고자 하였다.

- Metamorphic Mutation
    
    -인간의 관점에서 동일한 의미를 가진 생성하는데 있어서 metamorphic mutation이 얼마나 효과적인가?
    
- Coverage
    
    -무작위 테스트와 비교해서 CGF가 DNN 커버리지 향상에 얼마나 더 효과적인가?
    
- Error Detection
    
    -기존에 존재하는 기준이 잘못된 것을 가이드하는데 어떻게 차이를 보이는가?
    
    -시드 선택이 잘못된 것을 찾는데 어떠한 영향을 주는가?
    
    -서로 다른 전략에 따라 얼마나 다양한 잘못된 것을 탐지 할수 있는가?
    
- Platform Migration
    
    -DeepHunter를 통해서 DNN 양자화를 통해 발생하는 문제를 얼마나 잘 탐지 할 수 있는가?
    

## 기본적인 단어

### DNN Trating

기존에 존재하는 소프트 웨어는 프로그래머가 코드를 작성하면 그 코드 대로 실행을 하게 된다. 하지만 DNN 결정 로직은 프로그래머가 데이터와 라벨을 전달하면 스스로 학습을 진행하게 된다.

### DNN Quantization

DNN 양자화를 통하여 DL 모델의 저장 용량을 줄이고 사용하는 메모리 양을 줄일 수 있다. 하지만 그로 인해서 발생하는 문제들도 존재한다.

### DNN Testing Criteria

- Neuron Coverage(NC)
    
    출력 값이 threshold를 넘어서면 뉴런이 활성화가 된다.
    
- k-Multisecion Neuron Coverage(KMNC)
    
    범위 값을 k개의 구역으로 나눈뒤 각 뉴런에 할당한다. 출력값이 해당되는 뉴런이 활성화 된다.
    
- Neuron Boundary Coverage(NBC)
    
    최대값을 넘어서는 corner-case를 뉴런이 처리하는지 확인한다
    
- Strong Neuron Activation Coverage(SNAC)
    
    어떻게 upper corner-case regions의 뉴런이 커버 되는지 확인한다
    
- Top-k Neuron Coverage(TKNC)
    
    각 레이어에서 가장 할성화된 k개의 뉴런을 사용한다.
    

![DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled.png](DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled.png)

해당 사진은 DeepHunter가 어떻게 동작하는지를 보여준다.

![DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%201.png](DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%201.png)

해당 사진은 DeepHunter 테스트 생성 알고리즘 이다.

# Transformation and Mutation

인간의 관점에서 그 이미지가 유효하다는 것을 판단을 할 수 없다면 DNN 테스트에서 mutatior는 쓸모가 없어진다.

### Definition 1.

Mutation 하기 전의 oracle과 mutaion 한 후의 oracle이 같으면 우리는 그 mutation을 Metamorphic Mutaion이라고 정의를 한다.

### Definition 2.

변형 전에 예상치와 변형후의 예상치가 다르다면 그것은 erroneous behavior 이다.

### Definition 3.

하나의 모델에서 테스트의 예상치가 옳지만 다른 모델에서 틀리게 나온다면 그것은 quantization erroneous behavior이다.

보수적인 mutation strategy를 통해서 해당 논문에서는 의미상의 문제가 생기는 일로 인해서 발생하는 실패 가능성을 낮추었다.

### Pixel Value transformation

이미지 픽셀 값을 변화 시킨다

image contrast, image brightness, image blur, image noise

### Affine transformation

이미지를 이동 시킨다

image translation, image scaling, image shearing, image rotation

이미지의 의미를 최대한 오리지널 이미지와 동일하게 유지하기 위해서 Affine Transformation은 한번만 적용시킨다. Pixel Value Transformation은 이미지의 변화를 위해서 여러번 적용 될 수 있다.

### Definition 4.

이미지에 변화가 생길 때에는 연속적으로 이루어져야 한다.

![DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%202.png](DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%202.png)

여기서 $L_0$는 s 와 s' 사이에 변화한 픽셀들의 최대값을 나타내며, $L_\infty$는 변화된 픽셀의 최대값을 나타낸다. 변화된 픽셀의 개수가 적다면 픽셀은 아무 값이나 될 수 있지만, 변화된 픽셀 개수가 많다면 변화되는 값을 제한이 된다. Affine Transformation이 일어나게 된다면 픽셀은 1대1로 상응하는 연결이 사라지게 된다.

### Definition 5.

Affine Transformation(AT)이 있기 전까지는 reference image는 오리지널 이미지 이지만 AT 이후에는 reference image는 AT 직후의 이미지로 바뀌게 된다.

![DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%203.png](DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%203.png)

연속해서 mutation이 일어날 때 $L_0$와 $L_\infty$는 다음과 같은 방법으로 구할 수 있다. 이때  $t_{j-1}$ 은 Affine Trasformation 이다.

![DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%204.png](DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%204.png)

해당 알고리즘은 DeepHunter에서 mutation이 어떠한 방식으로 일어나는지에 대한 알고리즘이다.

## Seed Prioritization

Seed Prioritization은 시드큐에서 다음으로 어떠한 시드를 선택할지를 정하는 과정이다.  기존에 존재하던 CGF의 경우에는 새로 선택된 테스트를 큐에서 선택하는 것을 선호했다. 

DeepHunter의 경우에는 2가지의 seed selection strategies를 사용한다. 첫번째는 랜덤하게 큐에서 시드를 선택하는 것이다. 두번째는 시드가 몇회 퍼즈가 되엇는지를 기반으로 선택하는 전략이다.

![DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%205.png](DeepHunter%20Hunting%20Deep%20Neural%20Network%20Defects%20via%20b02363cd441b4a00ba474e381be4b7a3/Untitled%205.png)

$g(s)$ 는 시드가 몇회 포즈가 되었는지를 나타내며, $p_{min} > 0$ 의 경우에는 시드 선택의 최소 확률을 나타낸다. $\gamma$의 경우에는 감쇠 특성을 가지는 변수이다. 

DeepHunter는 다양성의 기반한 시드 선택과 최근 추가된 시드를 선택하는 전략중에 선택을 잘 조절한다. 

### Random Testing(RT) without coverage guidance

랜덤하게 선택하여 비교의 기준이 된다

### DeepHuner + Uniform (DH + UF)

여러가지 기준을 이용하여 RT를 지도한다.

### DeepHunter + Probability (DH + Prob)

확률에 기반한 시드 선택 전략을 상용한다

### DeepTest seed selection strategy with coverage guidance

가장 마지막 시드가 큐에서 선택이 된다. 새로 생성된 테스트가 커버리지를 향상 시켰을 경우 큐 끝단에 추가가 된다. 커버리지를 향상 시키지 못하면 큐에서 제거 된다. 만약 모든 테스트가 커버리지를 향상 시키지 못한다면 시드 큐는 비어 있을 수 있다.

### Tensorfuzz seed selection strategy with coverage guidance

시드 하나를 랜덤하게 선택한 뒤 m-1개의 시드를 큐의 rear에서 선택한다. 이렇게 선택된 시드들중 랜덤하게 시드를 다시 선택한다.  

## RQ1. User Study on Metamorphic Mutation

CIFAR-10의 경우에는 연관성이 낮아 인간이 구분하는데 어려움을 격게 된다. DeepHunter는 DeepTest나 TensorFuzz와 비교 했을 때 invalidity ratio가 낮게 나온다.

mutation strategy에서 파라미터 조정과 디자인이 invalidity image ratio를 줄이는데 많은 영향을 끼치는 것을 확인 할 수 있다.

## RQ2. Results of Coverage Increase

실험 결과를 보면 CGF가 랜덤하게 테스트 하는 것보다 커버리지를 극대화 하는데 효과적임을 보였다, 특히 커버하기 어려운 criteria에 대해서 나타났다.  이는 criteria가 숨겨진 의사결정 정보에 포함되어 coverage-guide 전략이 일반적인 랜덤 선택보다 커버리지 확장에 도움이 되었다고 볼 수 있다.

다양한 시드를 시드큐에서 샘플링 하는 것이 DNN 테스팅에서는 중요하다. Tensorfuzz와 DeepTest는 새로 생성된 테스트를 선택하는 것을 선호 한다. 이와 반대로 DH + UF 와 DH + Prob은 퍼징을 진행하는 동안 다양한 시드를 선택 하는 것을 선호한다. 추가적으로 DH + Prob은 랜덤 시드 선택 방식과 새로운 선택 방식을 잘 조절하여 사용한다.

## RQ3. Results of Error Detection

시드 선택의 다양성이 중요한것은 RQ2에 의해서 증명 되었다. 모델의 복잡도가 올라갈 수록 DH + Prob이 DH + UF 보다 더 많은 에러를 찾아낸다. 

주요 기능 습성 커버리지는 다른 기준보다 에러를 찾는 능력이 떨어진다.

모델이 커질 수록 퍼저는 더 많은 시드를 가지고 있으려고 한다. 더 많은 시드를 가짐으로서 큰 모델의 커버리지는 쉽게 좋아 질수 있기 때문이다.

다양한 에러는 개발자에게 많은 정보를 줌으로써 성능을 개선 시키는데 도움이 되는 문제를 이해하는데 도움을 준다.  DH + Prob 와 DH + UF는 다른 전략보다 더욱 다양한 에러를 찾았다. DeepTest와 TensorFuzz의 경우에는 RT보다 적은 양의 에러 카테고리를 발견했다. 그 이유는 두개 모두 시드 선택 전략에서 밸런싱 없이 새로운 테스트를 큐에서 선택 했기 때문에 다양성에서 문제가 발생 했기 때문이다.

새로운 시드를 선택 하는 것은 corner cases에 도달할 수 있겠지만 다양성에 부족함을 느낄수 있고, 랜덤 선택은 다양성을 증가 시켜줄 수 있겠지만, 코너 케이스를 찾지 못 할 수 도 있다. 따라서 이에 적절한 밸런싱이 중요하다.

## RQ4. Error Detection for Quantization

커버리지 가이드에 따라 DeepHunter가 찾아내는 에러의 숫자에는 차이가 생긴다. 

Corner-region 기반의 기준은 양자화에 의해 발생한 에러를 탐지하는데 더움이 된다.

같은 Quantization ration(QR)은 더 큰 모델에서 정밀함을 잃게 만들 것이고 결정 로직에 더욱 많은 변화를 일으킬 것이다. QR이 증가 함에 따라 DeepHunter와 TensorFuzz 모두 탐지 되는 에러의 수가 증가했다.
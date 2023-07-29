# Wild patterns: Ten years after the rise of adversarial machine learning

태그: Defend, for AI
읽은 날짜: 2021년 7월 13일
논문 파일: Wild%20patterns%20Ten%20years%20after%20the%20rise%20of%20adversar%209dadf716eb314c12936654f40494f20d/Wild_patterns-_Ten_years_after_the_rise_of_adversarial_machine_learning.pdf

# 1. Introduction

머신러닝이 adversarial examples에 의해서 쉽게 무력화가 될 수 있다.

2004년에 Dalvi 외 2인이 스팸메일의 내용을 가독성에 지장을 주지 않는 범위에서 살짝 변경하는 것으로 선형 분류기를 속일수 있다는 것을 알게 되었다.

학습 단계와 테스트 단계에서 categorizing attack을 막기 위해서 대응책을 마련하기 시작했다. 

1. 학습 단계(poisoning)와 테스트 단계(evasion)에서의 머신러닝을 공격할 수 있는 방법을 개발한다.
2. 해당 공격에 대한 학습 알고리즘의 보안 평가를 위한 체계적 방법을 제시한다
3. 이러한 위험을 해결할 수 있는 방어 메커니즘을 고안한다

머신러닝 보안에서 3가지를 중요시 한다. 따라서 사전 대응이 중요하다.

- 상대방을 알아라
- 사전대책을 강구해라
- 스스로를 보호해라

---

# 2. Arms race and security by design

보안은 경비 경쟁이다. 90년대 이후에 악성코드의 양 뿐만 아니라 다양한 종류의 악성코드가 생겨났다. 이러한 다양하고 복잡한 공격에 대응하기 위해서 머신러닝과 패턴인식은 사이버 보안 분야에서 많이 사용 되었다. 하지만 머신러닝이 이러한 문제들을 해결하는데 최종적인 정답은 아니었다. 몇가지 취약점을 악용하여 숙련된 공격자가 시스템을 손상시킬수 있습니다. 이는 머신러닝 자체만으로는 보안이 취약한 약점이 될 수 있음을 나타냅니다.

## The spam arms race

스팸 이메일은 주로 텍스트 형태를 사용하여 메시지를 전달하는데, 이는 룰 기반의 필터와 텍스트 분류기를 통해서 찾아 낼수 있다. 스팸 이메일은 내용을 애매하게 바꾸어서 스팸 이메일로 구분 되는 것을 피하고자 한다.

<img width="500" alt="Untitled" src="/public/img/2021-07-13/Untitled.png">

2005년에는 스팸 내용을 이미지로 전달하면서 스팸으로 분류 되는 것을 피하는 기법이 나왔다. 이에 대한 해결 방법으로는 해싱을 통해 스팸 이미지를 구분 하는 방법이나, OCR 툴을 사용하여 스팸 메시지를 읽는 방법이 고안 되었다. 이러한 방어 기법을 우회하기 위해서 이미지에 랜덤 노이즈 패턴을 넣는 방법이 생겨 났다. 이는 아이러니 하게도 CAPTCHA가 사용하는 방식과 같은 방식을 가지고 있다. 이러한 스팸 이미지를 탐지 하기 위해 저레벨 단계의 시각적 특징을 기반으로한 학습 기법이 연구되기 시작했다.

## Reactive and proactive security

<img width="500" alt="Untitled 1" src="/public/img/2021-07-13/Untitled%201.png">

1. 공격자는 방어 시스템을 분석하고 보안 취약점을 공격한다
2. 시스템 디자이너는 새로 발견된 공격 기법을 분석하고 대응책을 고안한다. 

하지만 reactive식 접근 방법은 이전에 본적 없는 공격을 막는것이 불가능하다. 따라서 사전에 공격자를 예상하여 대응책을 준비해야 한다.

<img width="500" alt="Untitled 2" src="public/img/2021-07-13/Untitled%202.png">

1. 관련 있는 위협을 파악하고, 해당 공격을 시뮬레이션 해본다
2. 해당 공격에 대응책을 생각한다
3. 시스템이 배치되기 전까지 이러한 과정을 반복한다.

이러한 프로세스를 통해서 학습 기반의 시스템을 대상으로하는 잠재적 공격 시나리오의 숫자와 계획을 파악하는데 도움이 된다.

---

# 3. Know your adversary: modeling threats

학습 기반 시스템에 대한 위협을 모델링하고 해당 공격에 대한 보안을 철저히 평가하는 방법을 설명한다. 공격 분류 체계에 기초한 프레임 워크를 활용하고 이를 통해 학습 알고리즘과 심층 네트워크에 대한 다양한 공격 시나리오를 구상하고, 해당 공격 전략을 구현할 수 있다. 이는 공격자의 목적과 타겟 시스템의 지식, 입력 데이터를 잘 다루는 능력, 이후 최적 공격 전략에 해당하는 최적화 문제를 정의한다. 이 프레임 워크는 지도 학습 알고리즘을 대상으로하는 공격만 다룬다.

## 3.1 Attacker's goal

$x$: sample 

$y$: label 

$D=(x_i , y_i)^n_{i=1}$: 학습 데이터

n: 학습 데이터 개수

$L(D, w)$: 분류기에 의한 손실

$f$: 분류 함수

$w$: 학습 파라미터

$\Theta$: attacker knowledge

$A(D'_c, \theta) \in\Re$: 얼마나 $D'_c$의 공격이 효과적인가

### Security violation

- 무결성 위반: 일반적인 시스템에 손상을 입히지 않으면서 탐지를 피한다
- 가용성 위반: 합법적인 사용자가 사용할 수 있는 일반 시스템을 손상 시킨다
- 사생활 침해: 시스템에 대한 개인적인 정보를 획득한다

### Attack specificity

타겟을 정하고 공격하는지 아니면 무차별 적으로 공격하는지 다양하게 나타난다.

### Error specificity

공격자가 오분류를 일으킬 때 특정 클래스로 오분류가 일어나게 만들 것인지 아니면 원본 클래스가 아닌 아무 클래스로든 상관없이 오분류가 일어나게 만들 것인지로 나뉜다.

## 3.2 Attacker's knowledge

공격자는 목표 시스템에 대한 다양한 수준의 지식을 가지고 있을 수 있습니다.

### Perfect-knowledge white-box attacks

공격자는 타겟 시스템에 대해 모든 것을 알고 있다고 가정한다. 이를 통해 알고리즘 보안에 대한 최악의 경우를 평가하고 수행할 수 있다. 또한 성능 저하에 대한 상한을 제공한다.

### Limited-knowledge (LK) gray-box attacks

공격자가 $x$의 특징과 학습 알고리즘의 종류를 알고 있지만, 학습 데이터 $D$ 와 학습된 분류기의 파라미터 $w$를 알지 못한다고 가정한다. 하지만 공격자가 대용의 데이터셋 $\hat D$을 비슷한 소스에서 얻을수 있다고 가정하고, 해당 데이터에 대한 라벨링을 분류기로부터 받을 수 있다고 가정한다. 대리 분류기를 통해 공격자는 $\hat w$를  $\hat D$를 통해서 유추할 수 있다. 이러한 케이스를 LK attacks with Surrogate Data(LK-SD)라고 하고 $\theta_{LK-SD}=(\hat D, x, f, \hat w)$을 나타낸다.

공격자가 학습 알고리즘 $f$ 의 종류에 대해서 모르는 것을 LK attacks with Surrogate Learners (LK-SL) 이라고 하고, $\theta_{LK-SL}=(\hat D, x, \hat f, \hat w)$을 나타낸다. 이는 공격자가 학습 알고리즘을 알지만 공격 샘플을 최적화 하는것이 불가능하거나 너무 복잡한 경우도 포함한다. 이러한 경우에는 공격자는 대리 분류에 대한 공격을 만들어 표적 분류기에 대해 테스트할 수 있다.

### Zero-knowledge (ZK) black-box attacks

공격자가 내부 정보를 알지 못하더라도 블랙박스 방식으로 시스템을 쿼리하고 제공된 라벨 또는 신뢰 점수에 대한 피드백을 얻을 수 있다면 공격이 가능하다. 

우선 공격자는 분류기가 어떠한 작업을 하기 위해 디자인 되었는지 알아야 하고 기능 변경을 유발하기 위해 적용할 잠재적 변환에 대한 생각이 있어야 한다.  정확한 특징을 공격자가 알지는 못하지만 어떠한 특징이 사용되는지 정도의 정보를 알거나 알게 된다. 즉 특징 표현에 대한 지식이 완전히 없는 것이 아닌 부분적으로 있게 된다. 또한 학습을 위해서 정확히 어떠한 데이터가 사용되는지 알지는 못하지만 어떠한 유형의 데이터가 사용되는지 알게 된다. 

## 3.3 Attacker's capability

### Attack influence

공격자가 training data와 test data에 둘다 접근 가능한 경우와, test data에만 접근이 가능한 경우로 나뉘게 된다

### Data mainipulation constraints

공격자의 애플리케이션별 데이터 조작에 대한 제약의 존재에 따라 달라진다. 

첫 공격 샘플인 $D_c$는 변경 가능한 $\Phi(D_c)$에 의해서만 수정될 수 있다.

## 3.4 Attack strategy

attacker's knowledge와 attack samples이 주어졌을 때 공격자의 목적이 얼마나 달성 되었는지 알게 해준다

object function ($A(D'_c, \theta) \in\Re$ )은 attacker's knowledge, 

## 3.5 Security evaluation

<img width="500" alt="Untitled 3" src="public/img/2021-07-13/Untitled%203.png">

학습 알고리즘의 보안 수치를 측정하기 위해서 공격자의 정보 수준 뿐만 아니라 공격 강도에 따라서도 측정을 해야한다.

## 3.6 Summary of attacks against machine learning

<img width="500" alt="Untitled 4" src="public/img/2021-07-13/Untitled%204.png">

위에 정리 했었던 공격기법들이 표로 정리 되어 있다.

가장 흔한 공격은 테스트 에러의 수를 증가시키는 회피 기술과 poisoning 기법이다.

<img width="500" alt="Untitled 5" src="public/img/2021-07-13/Untitled%205.png">

최근 들어서는 무결성 공격이 딥 네트워크에 적용이된다. 이는 악의적으로 결함이 있는 모델을 만들어 배포한뒤 이를 사용하 시스템이 생기면 계획된 오분류를 일으키는 데이터를 넣어서 백도어를 실행 시킨다.

공격자의 정보가 제한적일 때는 그레이 박스나 블랙 박스와 같이 분류자나 사용자의 정보를 얻기 위해서 공격이 진행될 수 있다.

---

# 4. Be proactive: simulating attacks

## 4.1 Evasion attacks

테스트 단계에서 입력되는 데이터를 잘못된 클래스로 분류 되도록 하는것이 목적이다.  이 공격은 classifier가 잘못된 클래스로 분류하는 비율을 증가시키는 것을 목저으로 하고 있다. 

$f_i(x)$는 샘플 x에 대한 i로의 분류에 대한 신뢰도를 뜻한다. 이러한 공격은 attacker's knowledge의 따라 최적화가 될 수 있다.

### Error-generic evasion attack

공격자는 예측한 출력 클래스와 상관없이 오분류 시키는 것 자체를 목적으로 한다. $f_k(x)$는 x가 클래스 k로 제대로 분류되었는가에 대한 판별식이고 $max_{l\not\equiv k}f_l(x)$이 가장 가까운 경쟁 클래스이다. 

- $d(x, x') \leq d_{max}$는 입력 데이터 x와 adversarial example인 x' 사이의 범위가 일정값을 넘을 수 없음을 나타낸다.
- $x_{lb} \leq x' \leq x_{ub}$로 공격 샘플 x'의 범위를 정한다.

이미지에서 $l_2$와 $l_\infin$은 구분되지 않게 이미지를 흐리게 만드는 효과를 가지고 있다. 그에 비해 $l_1$은 몇개의 픽셀만 현저하게 변조시키는 공격을 일으킨다.

### Error-specificevasion attacks

공격자는 특정한 클래스로 오분류를 시키려고 한다. 

- 판별식 $A(x', \theta) = - \Omega(x')$는 opposite sign을 가지고 있다
- $f_k$는 대상 클래스의 오분류가 정상적으로 이루어 졌는지를 판별한다

일반적인 상황에서 틀린 타겟 클래스인 $f_k$로의 분류를 최대화하면서 제대로 분류되는 것을 최소화 하는 것이 공격 목표이다.

### Attack algorithm

두개의 공격이 개념적으로 분류 되어 있지만 둘다 gradient-based attack을 사용한다.

### 4.1.1 Application example

adversarial examples에 대항하기 위해 SVMs, SVMs with RBF kernel, 그리고  간단한 방어 메커니즘을 학습했다. 

SVM-adv의 reject 메커니즘은 낮은 레벨의 perturbations에서 효과를 발위 하였다. 높은 레벨의 perturbations는 입력 이미지가 다른 물체와 닮은 것 과는 거리가 멀지만 대상 클래스의 샘플과 구별할 수 없다.

### 4.1.2 Historical remarks

linear classifier을 대상으로한 evasion attack은 이미 2010년부터 존재해 왔으며 간단한 heuristic countermeasure는 이미 개발이 진행 되고 있다.

nonlinear classifier의 경우에는 기존에는 linear classifier을 대상으로한 공격은 잘 방어했지만 nonlinear classifier를 대상으로한 공격에 대한 방어도 발전을 시켜야 한다는 결론에 이르렀다.

공격자가 타겟 classifier에 대한 완벽한 정보가 없더라도 surrogate

 classifier와 surrogate 학습 데이터를 이용하여 비슷한 정보를 얻을 수 있다.

### Security of deep learning

adversarial examples은 adversarial smaple x'과 해당 소스 샘플 x의 거리를 줄임으로써 얻을 수 있다.

$min_{x'}d(x, x') s.t  f(x)\neq f(x')$

오분류를 일으키는 작은 변화를 찾기 시작했다. 이는 배경 이미지나 이미지의 구조가 변경 될 것이라고 예상을 했지만 사람의 시각으로 보기에는 큰 차이가 없는 것으로 밝혀졌다.

distillation은 surrogate classifier를 기반으로하는 공격에 취약하다, 특히나 limited-knowledge evasion attacks에 취약하다.

### Misconceptions onevasion attacks

adversarial example이 최소한의 변형만 가해져야 한다는 것이 misconception이다. 공격자는 단지 공격 샘플에 변화를 적게 주는것을 목적으로 하는것이 아닌 출력 클래스의 신뢰도를 극대화 하는 것이 목적이다.

## 4.2 Poisoning attacks

학습 데이터에 감염된 샘플을 적은 비율 넣음으로써 잘못 분류되는 비율을 늘리는 것이 목적이다.

### Error-generic poisoning attacks

공격자는 어떤 클라스로 분류되던간에 상관없이 오분류를 일으켜서 서비스에 지장이 생기도록 만드는 것이 목적이다.

Outer optimization은 공격자의 목적 A를 극댈하 시키고 Inner optimization은 교란된 학습 데이터를 classifier에 학습 시킨다

$A(D_c',\theta)=L(D_{val}, w^*)$

$w^* \in arg minL(D_{tr}\cup D_c',w')$

이때 $D_{tr}$과$D_{val}$은 공격자에게 허용된 데이터셋이다. $D_{c}'$은 posined 된 데이터로 학습 시키는데 상용된다. 추후 loss function $L(D_{val}, w^*)$을 통해서 얼마나 효과적인지 측정한다.

### Error-specific poisoning attacks

공격자는 특정 클래스로의 오분류를 일으키고자 한다. 이때 loss function은 재정의 된다.

$A(D_c',\theta)=L(D_{val}', w^*)$

이때 $D_{val}'$은 $D_{val}$과 같은 데이터를 가지고 있지만 라벨이 공격자에 의해서 정해져 오분류를 일으키게 설계 되어있다.

### Attack algorithm

back-gradient poisoning: 자동적인 미분과 학습 절차를 리버싱 함으로서 기울기를 계산한다

### 4.2.1 Application example

back-gradient poisoning을 통해서 생성된 adversarial training examples은 랜덤으로 생성된 것 보다 더 오분류를 잘 일으키는 것으로 확인 되었다.

### 4.2.2 Historical remarks

머신러닝에 대한 poisoning attck은 학술적인 활동으로만 다루어져서는 안된다.

# 5. Protect yourself: security measures for learning algorithms

## 5.1 Reactive defense

이는 과거의 공격을 방어하기 위함이다. 대응하는 방식은 다음을 포함한다.

- 새로운 공격을 시기 적절하게 탐지한다
- classifier을 자주 재학습 한다
- 학습 데이터와 기존 라벨을 통해 classifier의 결정의 일관성을 검증

이러한 절차는 필요할때 보다 쉽게 작동할 수 있도록 자동화가 되어야한다.

## 5.2 Proactive defenses

이는 미래의 공격을 방어하기 위함이다.

### 5.2.1 Security-by-design defenses against white-box attacks

adversarial data를 학습 데이터에 포함하여 방어 시스템을 디자인한다.

### Countering evasion attacks

2004년에 시뮬레이션 공격에 반복하여 학습하는 것을 기반으로하여 evasion attack에 대응하는 adversary-aware classifer을 만들었다. 하지만 이런것은 covergence와 robustness에 대한 guarantee가 없다.

Game theory에 기반하여 위와 같은 문제를 해결하고자 한다. Zero-sum games은 변함없는 변환(특징 삽입, 삭제, 리스케일링)을 학습하기 위해 고안되었다. 

Adverasial learning은 게임과 같이 잘 정의된 룰이 존제하지 않는다. 즉 현실의 공격자의 objection function은 앞에서 가설과는 일치하지 않을 수 있다. 게임 이론으로의 접근의 다른 무제점은 큰 데이터셋과 높은 차원의 특징 공간이다. 이로 인해서 충분하게 공격을 대표할수 있는 숫자의 샘플을 만드는데 어려움이 생길 수 있다.

Robust optimization은 adversarial learning을 minimax 문제로 해결하고자 한다. 이는 inner problem은 트레이닝 포인트를 조종하여 트레이닝 손실을 최대화하고, outer problem은 worst-case의 training loss와 일치하는 것을 최소화 하는 방향으로 학습 알고리즘을 학습한다.

이는 regurized learning problems랑 robust optimization이 특정 사항과 structured learning에서 보안적으로 같은 효과를 나타낸다.  최근에는 이러한 특징을 nonlinear classifier에도 확장을 시키고 있다. 이를 통해서 worst-case input에 좀 덜 민감하게 변하도록 만든다.

학습 데이터로부터 너무 먼 샘플을 찾고 배제하는것 또한 새로운 대응 방법이다. 

classifier을 모으는 것이 방어 능력 향상에 도움이 된다고 이야기했다. 하지만 기본 classifier가 제대로 합쳐지지 않았다면 보안적으로 더욱 약할 수 있다.

### Effect on decision boundaries

training classes를 더욱 타이트하게 둘러싸도록 만든다.

클래스 사이에 noise-specific margin을 형성한다

seucure learning algorithm을 사용하면 blind-spot evasion samples에 대처할 수 있지만 feature vectors가 구분되지 않는 샘플을 사용하는 adversarial example은 대처할 수 없다.

기능의 보안은 추가적이고 중요한 요구 사항으로 간주되어야 합니다. Security of features은 판별 수단으로만 여기는 것이 아니라 속임에 강하고 부정 classifier evasion을 회피할수 있어야 한다.

Deep convolutional networks에서 발생하는 문제들의 대부분은 deep space으로의 input이 smoothness assumption of learning algorithms을 위반하여 발생한다. Deep sapce에 있는 adversarial examples은 다른 클래스에 있는 학습 데이터와 구분이 되지 않는다. 이를 해결하기 위해서는 더 깊은 layers of the network를 retraining 하거나 re-engineering 하는것 밖에 방법이 없다.

### Countering poisoning attacks

 Poisoning attacks은 outlier로 간주해야 하며, 데이터 전처리를 통해서 방어할수 있으며, robust learning을 이용해서도 방어가 가능하다.

### 5.2.2 Security-by-obscurity defenses against black-box attacks

Gray-box와 black-box를 방어하고자 만들어졌다.  정보를 공격자에게 숨김으로써 방어를 하고자 한다.

- 데이터 수집 시간과 위치를 랜덤하게 진행한다
- classifier를 reverse-engineer하기 어렵게 만든다
- 실제 classifier나 training data로의 접근을 제한한다
- classifier의 output을 랜덤화 하여 불완벽한 feedback을 공격자에게 준다

# 6. Conculusions and future work

 Unknown unknown이 위협이 되는데 이는 proactive 접근을 통해서 일부 해결 할수 있지만 여전시 unpredictable하다. 추후의 연구들은 unknown unknown을 robust methods를 통해서 탐지가 가능하도록 연구가 진행 되어야 한다.
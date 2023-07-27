# Diffusion 논문 정리

# Denoising Diffusion Probabilistic Models

## Introduction

### Diffusion

![Untitled](Diffusion%20%E1%84%82%E1%85%A9%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AB%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20ad814bd9be0542c991c6121d5e09ce75/Untitled.png)

Diffusion이란 확산을 의미한다. 특정 패턴이 서서히 사라져 가는 과정(분포도가 균일해지는)을 “Diffusion Process”라고 정의한다.

### Markov Chain

![Untitled](Diffusion%20%E1%84%82%E1%85%A9%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AB%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20ad814bd9be0542c991c6121d5e09ce75/Untitled%201.png)

Markov Chain이란 Markov 성질을 가지는 이산 확률 과정을 의미한다. 이때 Markov 성질은 특정 상태의 확률(t+1)은 오직 현재(t)의 상태에 의존한다는 것을 의미한다. 이산 확률과정은 이산적인 시간(0초, 1초, 2초, …) 속에서의 확률적 현상을 의미한다.

### Generative AI Overview

![Untitled](Diffusion%20%E1%84%82%E1%85%A9%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AB%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5%20ad814bd9be0542c991c6121d5e09ce75/Untitled%202.png)

Diffusion 모델은 분포에 대한 변분적 추론을 통한 학습을 진행한다는 면에서 VAE와 유사하고 반복적인 변화를 활용한다는 점에서 Flow-based models와 유사하다.

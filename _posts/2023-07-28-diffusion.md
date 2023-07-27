---
layout: post
title: "Diffusion 논문 정리"
date: 2023-07-28 00:01:10 +0900
categories: Paper Summary
---

# Denoising Diffusion Probabilistic Models

## Introduction

### Diffusion

<img width="926" alt="Untitled 1" src="https://github.com/Jawoos/Jawoos.github.io/assets/49528515/41257df6-791e-46f9-8985-ecfd5b802d6c">

Diffusion이란 확산을 의미한다. 특정 패턴이 서서히 사라져 가는 과정(분포도가 균일해지는)을 “Diffusion Process”라고 정의한다.

### Markov Chain

![Untitled 2](https://github.com/Jawoos/Jawoos.github.io/assets/49528515/caf52981-613e-4e8a-927e-38d333f35d7a)

Markov Chain이란 Markov 성질을 가지는 이산 확률 과정을 의미한다. 이때 Markov 성질은 특정 상태의 확률(t+1)은 오직 현재(t)의 상태에 의존한다는 것을 의미한다. 이산 확률과정은 이산적인 시간(0초, 1초, 2초, …) 속에서의 확률적 현상을 의미한다.

### Generative AI Overview

![Untitled](https://github.com/Jawoos/Jawoos.github.io/assets/49528515/b9644631-2265-49eb-8f19-c4e3b3032c58)

Diffusion 모델은 분포에 대한 변분적 추론을 통한 학습을 진행한다는 면에서 VAE와 유사하고 반복적인 변화를 활용한다는 점에서 Flow-based models와 유사하다.

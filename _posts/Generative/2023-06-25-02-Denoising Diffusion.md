---
title: "[Paper] Denoising Diffusion Probabilistic Models"
toc: true
toc_sticky: true
toc_lable: "Main Contents"
use_math: true
categories:
  - Generative
tags:
  - []
---

"Denoising Diffusion Probabilistic Models"이란 논문에 대한 리뷰입니다.

원문은 [링크](https://proceedings.neurips.cc/paper/2020/hash/4c5bcfec8584af0d967f1ab10179ca4b-Abstract.html)에서 확인할 수 있습니다.


Reference

[Link 1](https://learnopencv.com/denoising-diffusion-probabilistic-models/#What-Are-Diffusion-Probabilistic-Models?--)

# Introduction

> A diffusion probabilistic model (which we will call a “diffusion model” for brevity) is a **parameterized Markov chain** trained using **variational inference** to produce samples matching the data after finite time.
> 

> … learned to **reverse a diffusion process**,$q(\mathrm{x}_t|\mathrm{x}_{t-1})$ which is a Markov chain that gradually **adds noise** to the data **in the opposite direction** of sampling until signal is destroyed.
> 
> 
> ![Untitled](/assets/images/generative/Untitled.png){: width="50%" height="40%"}{: .align-center}
> 

> When the diffusion consists of small amounts of Gaussian noise, it is sufficient to set the sampling chain transitions to conditional Gaussians too, allowing for a particularly simple neural network parameterization.
> 

![Untitled](/assets/images/generative//Untitled%201.png)

# Background

처음 diffusion에서 제시된 개념중 중요한 것은 

- Noise가 추가되는 과정 : forward trajectory(diffusion process)
- Denoising : reverse process
- 이 때 Gaussian Markov process가정
- (diffusion rate $\beta_t$가 작다면) 두 process는 동일한 distribution을 가지게 된다.

## Forward Process

### Initial and Final State

초기 값은 WLOG,  시작점을 기존의 데이터에서 하나 sampling해서 가져온다고 생각하자!

마지막 상태는 결과적으로 충분히 큰 step을 거쳤을 때 Gaussian noise와 같은 이미지가 된다.

- 이러한 step을 관리하도록 하는게 결국 diffusion rate이다.

### Single Step

![Untitled](/assets/images/generative/Untitled%202.png)

diffusion process는 $q(\mathrm{x}_{t}| \mathrm{x}_{t-1})$로 보게 된다.

- Forward diffusion kernel(process): $q(\mathrm{x}_{t}| \mathrm{x}_{t-1})$
    - 결과적으로 원래 이미지에서 점차 Gaussian noise가 누적되어서 곱해진 형태가 된다는 것
    - 그리고 이것은 neural net paramterized가 아닌게 그냥 Gaussian Noise가 그렇게 누적해서 곱해진 형태로 계산하면 된다.
    - 특히 variance가 diagonal해서 isotropic Gaussian이라고도 불린다.
    - **[***]** 중요한 것은 여기서 noise가 정의된 것을 Normal distribution의 성질을 이용해서 다음과 같이 정의할 수 있다. 이는 kernel의 정의를 decompose해보면 알 수 있다.
        
        > $x_t = \sqrt{1-\beta_t} x_{t-1} + \sqrt{\beta_t}\epsilon, \epsilon \sim \mathcal{N}(0,I)$
        > 

### Full Step

그리고 diffusion rate가 충분히 작을 때엔 결국 두 transition probability는 동일한 functional form을 가지게 되어서 forward process는 다음과 같이 closed form으로 정의된다고 한다.

![Untitled](/assets/images/generative/Untitled%203.png)

- $\alpha_t := 1-\beta_t$
- $\bar{\alpha}_t := \prod_{s=1}^t \alpha_s$,
- 여기서 closed form의 의미는 Markov process를 반복해서 얻을 결과를 미리 계산해서 가질 수 있다는 것이다.
- [***] 그리고 앞서 transition equation을 full step이 지난 이 시점에서 Closed form으로 작성하면 다음과 같다.
    
    > $x_t = \sqrt{\bar{\alpha}_t} x_{0} + \sqrt{1-\bar{\alpha}_t}\epsilon$
    > 

## Reverse process

### Initial and final state

초기 상태는 process가 누적된 것으로 정의하면 된다.(Jascha Sohl-Dickstein et al., 2015)

> $p_\theta(x_0) = \int p_\theta(x_{0:T})dx_{1:T}$
> 

최종 상태는 앞서 Gaussian noise로 생성된 이미지까지 만든다고 했으니 다음과 같이 정의된다.

> $p(x_T) := \mathcal{N}(x_t; 0,I)$
> 

### Single Step

![Untitled](/assets/images/generative/Untitled%204.png)

reverse process 는 $p(\mathrm{x}_{t-1}| \mathrm{x}_{t})$로 볼 수 있게 된다. 

- Reverse diffusion kernel(process) : $p(\mathrm{x}_{t-1}| \mathrm{x}_{t})$
    - 특히 이는 neural net으로 parameterization이 된다는 것을 기억하자!
    - 여기서 parameter들이 어떻게 계산되는지는 아래의 proposal을 보도록 하자.
- **[***]** 다만 여기서 transition equation은 다음과 같이 정의된다.
    
    > $\bar{x}_{t-1} = \mu_\theta(x_t,t) + \sqrt{\Sigma_\theta(x_t,t)}\epsilon$
    > 

### Loss

기본적으로 Maximization of Log Likliehood를 하려고 한다. 그리고 VAE와 같이 lower bound를 maximization(descent algorithm에서는 upper bound를 minimization)하는 방식으로 loss function을 정의하게 된다. 

![Untitled](/assets/images/generative/Untitled%205.png)

그리고 equivalent한 Loss function를 다음과 같이 정의할 수 있게 된다.

![Untitled](/assets/images/generative/Untitled%206.png)

- 이 식을 사용하는 이유로 variance reduction이라고 하게 되는데 다음의 과정으로 유도된다.(See Derivation)
- 위의 결과는 결국 각 step에서의 metric의 합으로 나타나게 된다.
- 중간 상태에서의 항은 흔히 a.k.a Forward process posterior distribution
- 여기서 최종 상태에서의 loss는 무시가 가능한게, 우선 최종 상태에 대한 neural net의 parameter는 존재하지않는다. 이는 parameterization이 transition equation에서 사용된 반면 다음 상태가 존재하지 않는다면 모델이 계산할 필요도 없으니 자명하다.
- 초기 상태의 loss는 실험적으로 더 좋은 결과를 얻었다고 한다.
- Derivation
    
    ![Untitled](/assets/images/generative/Untitled%207.png)
    
    사실 위의 식은 계산상으로는 가능하지만   해석을 위해서 이와 같은 식의 전개를 사용할 수 있게 된다. 
    
    ![Untitled](/assets/images/generative/Untitled%208.png)
    

이 때 tractability는 다음 정의로 보장된다고 한다. 

![Untitled](/assets/images/generative/Untitled%209.png)

- 이건 아마 VAE와 같이 derive를 하면 구할 수 있을 것 같다.

그리고 바로 사용하는 것이 아니라 구현하기에 쉽고 유용한 형태의 loss만을 사용하도록 한다.

![Untitled](/assets/images/generative/Untitled%2010.png)

- **[***]** 중요한 것은 이 loss를 다 사용하는 것이 아니라 유의미한 항만을 사용하도록 한다.
- 이에 대한 유도는 아래의 Paramterization을 보도록 하자.
- **[**]** 핵심은 원래 유도된 식에서 coefficient로 ${\beta_t^2\over 2\sigma_t^2 \alpha_t(1-\bar{\alpha}_t)}$가 사라지게 된다. 이에 대한 해석으로 각 step별로 있어서 최종 Loss에 대한  Weight를 무시하게 된 것이고(t에 대한 dependency가 존재하니) 특히 t가 작을 때 Weight를 줄인 효과가 있게 된다. 이는 처음에 denoising할 때 복잡한 표현에 대한 것은 large t에서 진행되도록 하게 하는 효과가 있다.
- 여기서 또하나의 특징은 아래의 유도과정을 통해서 이 식을 얻고 이 식을 계산한다는 것의 의미는 단순히 매 단계에서 noise에 대한 예측을 하게 된다는 것이다. 결국 mean square error!

# Proposal : Diffusion models and denoising autoencoders

앞서 Background로 제시된 페이퍼는 크게 2가지의 argument를 가지는 class of function을 제시한 것이다.

- $\beta_t$
- Parameter estimation architecture for Gaussian Noise

이들을 어떻게 다루었는지 보도록 하자

### Diffusion rate for forward process

실제로 학습으로 계산할 것이 아니라 fixed constant로 사용하도록 한다. 

![Untitled](/assets/images/generative/Untitled%2011.png)

그리고 앞서 transition equation을 정의한 것을 이용해서 converge할 때 즉  Gaussian image가 만들어질 때까지 반복하도록 한다.

### Parameterization for reverse process

앞서 reverse process를 정의한 것만 보았을 때 분포의 parameter를 결정해야할 필요가 존재하며 다음의 세팅을 사용하도록 한다.

- $\Sigma_\theta(x_t,t) = \sigma_t^2 \mathrm{I}$.
    - 실험적으로 다음의 두 값을 사용한다고 한다. 여기서 diffusion rate 또한 상수이기에 이 값도 상수
        - $\beta_t$
        - $\tilde{\beta}_t = {1-\bar{\alpha}_{t-1}\over 1-\bar{\alpha}_{t}} \beta_t$
- $\mathrm{x}_0$는 reverse process에서 entropy의 bound를 가지게 하는 값에 해당하는 것을 사용한다고 한다.
    - $\mathrm{x}_0 \sim \mathcal{N}(0,\mathrm{I})$
    - $\mathrm{x}_0$ to be set to one point(가지고 있는 데이터 셋에서 하나 sampling해서 시작하는 것)
- $\mu_\theta$는 다음 과정을 바탕으로 정해지게 된다.
    - Details
        
        ![Untitled](/assets/images/generative/Untitled%2012.png)
        
        - $\tilde{\mu}_t$ : forward process posterior mean(Loss section 확인해보기)
        
        이 식은 다음식에서 유도가 되었으며 크게 2가지의 식에 기반한다
        
        - $\mathrm{x}_t(\mathrm{x}_0,\epsilon) = \sqrt{\bar{\alpha}}\mathrm{x}_0 + \sqrt{1-\bar{\alpha}_t}\epsilon$
        - Equation 7
        
        ![Untitled](/assets/images/generative/Untitled%2013.png)
        
        - 기존의 식은 $D_{KL}(q(\mathrm{x}_{t-1}| \mathrm{x}_t, \mathrm{x}_0)\Vert p_\theta(\mathrm{x}_{t-1}|\mathrm{x}_t))$.
        - 이 때 $p_\theta(\mathrm{x}_{t-1}|\mathrm{t}) = \mathcal{N} (\mathrm{x}_{t-1}; \mu_\theta(\mathrm{x}_t,t),\sigma_t^2\mathrm{I})$.
        - 여기서 covariance term을 지우기 위해서 위에서 그냥 실험적으로 $\sigma_t^2$를 정한 것
        
        ![Untitled](/assets/images/generative/Untitled%2014.png)
        
        - 이 때 Full step의 transition equation을 사용 : $x_t = \sqrt{\bar{\alpha}_t} x_{0} + \sqrt{1-\bar{\alpha}_t}\epsilon$
        - 그리고 posterior mean에 대한 식 사용 : $\tilde{\mu}_t (\mathrm{x}_t,\mathrm{x}_0) := {\sqrt{\bar{\alpha}_{t-1}}\beta_t \over 1-\bar{\alpha}_{t}}\mathrm{x}_0 + {\sqrt{\bar{\alpha}_{t}}(1-\bar{\alpha}_{t-1})\over 1-\bar{\alpha}_{t}}\mathrm{x}_t$
        
        결과적으로 $L_{t-1}$은 다음과 같이 정리된다.
        
        ![Untitled](/assets/images/generative/Untitled%2015.png)
        
        - 여기서 주목할 점은 원래 posterior mean에 대한 식에서 noise에 대한 식으로 바뀌게 된다는 것이다.

중요한 것은 앞서 정의한 loss가 이 파라미터들을 학습 바탕으로 추정하는 것과 equivalent statement인지 확인해야 한다. 

우리가 해야하는 것은 결국 reverse process를 따라서 원래의 이미지로 돌아가야 한다. 결과적으로  다음 알고리즘으로 추론하게 된다.

![Untitled](/assets/images/generative/Untitled%2016.png)
---
layout: post
title: 나이브베이즈 분류모형 with Python
category: Statistics
tags: [naive bayse]
---

- NaiveBayse 는 언제 사용?  
  확률변수 A,B가 독립이면 A,B의 결합확률은 주변확률의 곱과 같다.  
    P(A,B) = P(A)P(B)  
  조건부독립 (conditional independence) 은 일반적인 독립과 달리 조건이 되는 별개의 확률변수 C가 존재해야 한다. 조건이 되는 확률변수 C에 대한 A,B의 조건부확류의 곱과 같으면 A와 B가 C에 대해 조건부독립이라고 한다.    
    𝑃(𝐴,𝐵|𝐶)=𝑃(𝐴|𝐶)𝑃(𝐵|𝐶)  
  
  - 나이브 가정  
    독립변수 x가 D차원이라고 가정하자.  
  		𝑥=(𝑥1,…,𝑥𝐷)  
    가능도함수는 𝑥1,…,𝑥𝐷의 결합확률이 된다.  
    		𝑃(𝑥∣𝑦=𝑘)=𝑃(𝑥1,…,𝑥𝐷∣𝑦=𝑘)  
    원리상으로는 y=k인 데이터만 모아서 이 가능도함수의 모양을 추정할 수 있다. 하지만 차원 D가 커지면 가능도함수의 추정이 현실적으로 어려워짐. 따라서 나이즈베이즈 분류모형(Naive Bayes classification model)에서는 모든 차원의 개별 독립변수가 서로 조건부독립(conditional independence)이라는 가정을 사용한다. 이러한 가정을 나이브 가정 (naive assumption) 이라고 한다. 나이브 가정으로 사용하면 벡터 x의 결합확률분포함수는 개별 스칼라 원소 𝑥𝑑의 확률분포함수의 곱이 된다.  
  - 정규분포 가능도 모형  
    x 벡터의 원소가 모두 실수이고 클래스마다 특정한 값 주변에서 발생한다고 하면 가능도분포로 정규분포를 사용한다. 각 독립변수 𝑥𝑑마다, 그리고 클래스 k마다 정규 분포의 기댓값 𝜇𝑑,𝑘, 표준 편차 𝜎2𝑑,𝑘 가 달라진다. QDA 모형과는 달리 모든 독립변수들이 서로 조건부독립이라고 가정한다.
  
- 나이브베이즈 코드  
[`GaussianNB`](http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html): 정규분포 나이브베이즈
  
  - 예제  
    붓꽃 분류문제를 가우시안 나이브베이즈 모형을 사용하여 풀어보자.
    (1) 각각의 종이 선택도리 사전확률을 구하라.
    (2) 각각의 종에 대해 꽃받침의 길이, 꽃받침의 폭, 꽃잎의 길이, 꽃잎의 폭의 평균과 분산을 구하라
    (3) 학습용 데이터를 사용하여 분류문제를 풀고 다음을 계산하라
    - 분류결과표
    - 분류보고서
    - ROC커브
    - AUC  
      
  
- **var_smoothing** : float, optional (default=1e-9)  
  Portion of the largest variance of all features that is added to variances for calculation stability.
 
- 스팸필터링 계산, 라플라시안이 들어가는 개념까지
  
- 참고사이트
    - https://datascienceschool.net/view-notebook/c19b48e3c7b048668f2bb0a113bd25f7/
    - https://chrisalbon.com/machine_learning/naive_bayes/gaussian_naive_bayes_classifier/
    - https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html
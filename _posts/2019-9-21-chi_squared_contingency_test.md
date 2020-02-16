---
layout: post
title: 카이제곱 검정 with Python
category: Statistics
tags: [chi-squared-distribution]
---

카이제곱 검정에 대해 정리해보고 python 코드를 작성했습니다.
<br><br><br>


- 카이제곱 검정은 언제 사용?  
  통계량으로 카테고리분포 표본의 합은 이용할 수 없다. 왜냐하면 이 통계량은 스칼라가 아닌 벡터값을 가지기 때문이다. 이 때 카이제곱검정 (Chi-squared test) 방법을 사용한다. 또한 어떤 범주형 확률변수 X가 다른 범주형 확률변수 Y와 독립인지 상관관계를 가지는가를 검증하는데도 사용할 수 있다. 확률 변수 X와 Y가 독립이라면 결합 확률질량함수 P(x,y)는 각 확률변수 X와 Y의 주변 확률밀도함수 P(x), P(y) 의 곱이다. 이런 확률변수의 표본을 측정하여 그 횟수를 표로 나타낸 것을 분할표 (contingency table)라고 한다. 

- 연습 문제  
  데이터 사이언스 스쿨 수업을 들었는가의 여부가 나중에 대학원에서 머신러닝 수업의 학점과 상관관계가 있는지를 알기 위해 데이터를 구한 결과가 다음과 같다. 이 결과로부터 데이터 사이언스 스쿨 수업을 들었는가의 여부가 머신러닝 수업의 학점과 상관관계가 있다고 말할 수 있는가?
  - 데이터 사이언스 스쿨 수업을 듣지 않은 경우 즉, X가 0이면 A,B,C 학점(Y값)을 받은 학생의 분포는 4, 16, 20
  - 수업을 들은 사람의 경우 X가 1이면, A,B,C 학점 (X값)을 받은 학생의 분포가 23, 18,19다

- 카이제곱 검정 코드

  ```python
  import numpy as np
  from scipy.stats import chi2_contingency
  
  
  obs = np.array([[4,16,20], [23, 18, 19]])
  chi2_contingency(obs)
  
  #(9.910060890453046, 0.00704786570249751, 2, 
  # array([[10.8, 13.6, 15.6], [16.2, 20.4, 23.4]]))
  ```

  아래 주석부분은 카이제곱 검정결과를 나타낸다. 4개결과값이 나오는데 각각 chi2, p, dof, expected 이다. 여기서는 2번째 pvalue , 카이제곱검정의 유의확률은 0.7 %다. 유의수준보다 작아 귀무가설을 기각한다.

- 카이제곱으로 확인할 수 있는 범주형의 크기

  위의 예제는 문제자체에 범주별 값(코드 상 obs)이 주어져있다. 이를 contingency 테이블 이라 하는데 실제 데이터셋에선 깔끔하게 이 테이블이 주어지지 않을 때가 있다. 데이터 세트에서 직접 contingency 테이블을 생성하는 것부터 시작인데, 이때 사용 가능한 것이 판다스의 크로스탭(crosstab) 이다. 처음 카이제곱 검정을 사용할 때 무턱대고 데이터의 특정 컬럼 데이터 전체를 바로 검정에 넣은 적이 있는데 아래와 같은 에러 코드를 볼 수 있었다.

  ```python
  import pandas as pd
  
  n = 20  # Number of samples.
  d = 3  # Dimensionality.
  c = 2  # Number of categories.
  data = np.random.randint(c, size=(n, d))
  data = pd.DataFrame(data, columns=['CAT1', 'CAT2', 'CAT3'])
  data # 20 X 3 dataset
  
  chi2_contingency(data['CAT1'], data['CAT2'])
  # ValueError: The internally computed table of expected 
  # frequencies has a zero element at (1,).
  ```

  ```python
  # Contingency table.
  contingency = pd.crosstab(data['CAT1'], data['CAT2'])
  
  # Chi-square test of independence.
  c, p, dof, expected = chi2_contingency(contingency)
  ```

  데이터 세트의 컬럼 전체를 넣게 되면 계산을 하지못한다. 판다스의 크로스탭을 사용해 확률변수의 표본을 측정하여 그 횟수를 표로 나타낸 분할표를 작성해서 입력값으로 넣으면 정상적으로 검정값이 나온다.

- 참고사이트  
  [9.5 사이파이를 사용한 검정, 데이터사이언스 스쿨](https://datascienceschool.net/view-notebook/14bde0cc05514b2cae2088805ef9ed52/)  
  [chi2_contingency, Scipy 참고문서](https://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.chi2_contingency.html)  
  [파이썬 contingency 테이블 만드는 방법, stackoverflow](https://stackoverflow.com/questions/24767161/can-we-generate-contingency-table-for-chisquare-test-using-python)  

  
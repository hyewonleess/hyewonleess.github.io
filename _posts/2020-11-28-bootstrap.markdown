---
title:  "[데이터분석이론] #2 Bootstrap"
categories:
  - Theory
tags:
  - bootstrap
toc: true  
sitemap: 
use_math: true
---

오늘은 **Bootstrap** 에 대하여 공부해볼 것이다. 이 포스트에서는 모델링에서의 bootstrap이 아닌, 모수 $\theta$ 를 추정할 때 사용하는 bootstrap에 대해서 다룬다.
<br>
<br>
**사용언어: R** <br>
Code link: <https://github.com/hyewonleess/github_blog_posts/tree/main/bootstrap>

## 1. Bootstrap 개념
먼저 미지의 모집단 F에서 관측 표본 $x_{1},x_{2},...,x_{n}$ 을 얻었다고 가정한다. 여기서 우리가 관심을 가질 것은 모집단의 모수 중 하나인 $\theta$ 이다. 이 $\theta$ 를 추정하기 위해서 관측 표본에서 n개의 표본을 sampling(with replacement)를 하여 재표본을 뽑는다. 이 재표본을 $x_{1k},...,x_{nk}$ 이라 하자. Bootstrap은 재표본을 여러번 sampling 하고 각 sampling 마다 표본에서 추정 모수 $\hat{\theta}$를 구하는 과정으로 이루어진다.  <br>

**Bootstrap 과정**
 1. 관측 표본  $x_{1},x_{2},...,x_{n}$ 에서 모수 $\theta$ 를 추정하는 통계량  $\hat{\theta} = T(x_{1},...,x_{n})$ 를 계산한다.
 2. $x_{1},x_{2},...,x_{n}$ 에서 sampling with replacement를 하여 $x_{1k},...,x_{nk}$ 를 구성하고, $x_{1k},...,x_{nk}$ 를 이용해 $\hat{\theta_{new}}= T(x_{1k},...,x_{nk})$ 를 계산한다.
 3. 위 작업을 B (boostrap 횟수) 만큼 반복한다.
 4. 각 B 에서 추정한 모수 $\theta$ 의 평균을 내 $\theta$ 의 boostrap 추정량을 구한다. $\hat{\theta_{boot}}=\frac{1}{B}\sum\hat{\theta_{new}}$ <br>
 그리고 $\hat{\theta_{boot}}$의 bias와 신뢰구간을 구한다.
 
**추정모수의 편향(bias)와 신뢰구간 구하는 방법**
 + Bias: $\hat{\theta_{boot}}-\hat{\theta}$ 
 + 신뢰구간: 
   - 백분위수방법(percentile method): 모수의 분포가 정규분포를 따를 때 적합한 방법. 예를 들어 90% 백분위수 신뢰구간을 구할 때, 하위 5%, 상위 5%가 신뢰구간의 기준이 된다.
   - BCa 방법(bias-corrected and accelerated method): 모수의 분포의 skewness가 클 때 사용하는 방법이다. 우리가 일반적으로 아는 신뢰구간을 구하는 방법이다.($\alpha$% 신뢰구간)
   
## 2. Bootstrap 예시
### (1) 상관계수 추정
두 변수 X, Y의 상관계수를 추정하는 bootstrap 예시를 살펴보자. 변수 X와 Y는 아래와 같이 길이가 20인 벡터로 정의한다.
```r
x <- c(15,26,10,9,15,20,18,11,8,20,7,9,10,11,11,10,12,17,11,10)
y <- c(95,71,83,91,102,87,93,100,104,94,113,96,83,84,102,100,105,121,86,100)
```
R 에서는 bootstrap 을 진행할 수 있도록 `boot` 패키지를 제공한다. 이 패키지를 이용하여 bootstrap 추정을 한다.<br>
우선 correlation을 계산하는 함수를 정의해야한다. `boot` 함수의 `statistics` 파라미터에 들어가는 함수이다. 그리고 여기서 R, 즉 bootstrap 반복 횟수는 1000으로 설정했다.

```r
# correlation 함수 정의
cor.X <- function(data,indices) cor(data[indices,1],data[indices,2])
# bootstrap 
boot.cor <- boot(data=cbind(x,y),statistic = cor.X,R=1000)
```
`print` 함수를 이용하면 bias와 standard error을 출력할 수 있다. 그러면 다음과 같이 결과가 나온다. Bias는  0.0347, standard error은 0.2869 로 나온다.
```r
print(boot.cor)
```
신뢰구간을 구하기 위해서는 `boot.ci` 함수를 사용하면 된다. 이 때 `type` 에서 'prec'은 백분위수방법, 'bca'는 bca 방법을 나타낸다. 
```r
boot.ci(boot.cor,type=c('prec','bca'))
```
이번 예시에서는 과연 어떤 신뢰구간이 적합할까? 이를 알아보기 위해서 각 bootstrap에서 구한 1000개의 correlation의 분포를 살펴보자.<br>
Histogram을 그려보면 correlation의 분포가 skewed 된 것을 확인할 수 있다. 따라서 BCa 신뢰구간이 더 적절할 것이다. <br>
**BCa 95% 신뢰구간: (-0.7801,  0.3607 )**
![hist](\assets\cor_hist.png)
  

### (2) 로지스틱회귀계수 추정
**Affairs** 데이터를 이용해 로지스틱 회귀계수를 추정하는 예시이다. Affairs 데이터에는 binary 변수가 없는데, 여기서 `ynaffair` 이라는 affair 의 yes/no 여부를 나타내는 이항 변수를 만든다.
`affair` 이 0보다 크면 `ynaffair`에 1을, `affair`이 0이면 `ynaffair`에 0을 부여한다.
```r
data(Affairs,package='AER')
Affairs$ynaffair[Affairs$affairs==0] <- 0
Affairs$ynaffair[Affairs$affairs>0] <- 1
```

이제 앞서 했던 예시처럼 로지스틱 회귀계수를 구하는 함수를 정의한다. R에서 로지스틱회귀분석을 할 때는 `glm` 함수를 사용한다. `family` 에는 반응변수가 binary이므로 binomial()을 써준다.
```r
coefs <- function(data,indices,formula){
  data.1<-data[indices,]
  fit <- glm(formula,family=binomial(),data=data.1)
  return(coef(fit))
}
```
이제 준비가 다 되었으니 bootstrap을 진행한다. Bootstrap 횟수는 1000번이고, formula에는 추정하고자하는 회귀식을 넣어준다. 코드 밑에 출력되는 결과가 있다.
```r
log.boot <- boot(data=Affairs,statistic=coefs,R=1000,formula=ynaffair~gender+age+yearsmarried+religiousness+rating)
print(log.boot,index=1:6,digits=2)
```
![result](\assets\boot_result.PNG)


그리고 신뢰구간을 구해준다. 코드는 위와 비슷하다. 여기서 특이한 점은 `index` 라는 입력값을 받는다는 것이다. 위 bootstrap 결과를 보면, t1부터 t6까지의 값들이 나와있는데 차례대로 intercept, 
gender, age, yearsmarried 이렇게 나타낸다. 그럼 index=2 라는 것은 두번째, 즉 gender의 로지스틱회귀계수의 신뢰구간을 구한다는 뜻이다. <br>
```r
boot.ci(log.boot,type=c('perc','bca'),conf=0.95,index=2)
```
![result](\assets\confidence.PNG)

---

이렇게 bootstrap의 개념과 R 코드를 활용한 예시를 살펴보았다. 이는 이전 포스팅에서 설명한 missing data imputation 방법으로도 사용할 수 있다. 다음에는 Bootstrap imputation 에 대한 포스팅으로 돌아오겠다!
<br>
<br>

**참고자료** <br>
<br>
*응용데이터분석 혀명회 저*

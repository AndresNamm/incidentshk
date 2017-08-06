---

category: LDA
---


{% include toc %}


# LDA 

## BAYES STATISTICS

LDA is a unsupervised learning method which originates from Bayes Statitistics. 


### BAYES FORMULA 

**General  Shape of bayes formula in probability Theory**
$$P(A|B)=\frac{P(B|A)P(A)}{P(B|A)P(A)+P(B|\neg A)P(\neg A)}$$

[Thorough tutorial into Bayes rule in Probability Theory - includes likelihood ratio](https://arbital.com/p/bayes_frequency_diagram/?l=55z&pathId=20114)

**An example of this would be to replace variables like this:**

+ A - Is sick   
+ B - Test shows sick.   
+ Then we would be asking the probability of somebody being sick if the test is showing sickness. $$P(A|B)$$.

### BAYES FORMULA IN BAYESIAN INFERENCE 
In bayes statistics we suppose that there is a distribution over $$\theta$$. In bayesian statistic $$\theta$$ varies. We assume that the data is fixed. 

**In statistics, the main aim of bayesian rule** is to calculate the probability of theta- parameter/s based on the given data. We often do this incrementally. E.g. we are improving/deepening our belief in certain parameters as the amount of data "goin through" our calculations grows. 


$$\stackrel{\text{Posterior}}{P(\theta|data)}=\frac{\stackrel{\text{Likelihood}}{P(data|\theta)} \stackrel{\text{Prior}}{P(\theta)}}{\stackrel{\text{Marginal}}{P(data)} }$$



+ **All the elements in the above formula are dependent on a [MODEL we choose](https://www.youtube.com/watch?v=i567qvWejJA&list=PLFDbGp5YzjqXQ4oE4w9GVWdiokWB9gEpm&index=15)**.The model becomes important for the margin to explain what the probability of data is. It is kinda probability of the data based on the model and chosend parameters. 
+ **Likelihood - It is not a probability, it does separately not integrate to 1** - This makes intuitively sense, because all parameters together for the same data produce multiple views and these vies cumulatively dont form a full probability. However if we include the beliefs to parameters this becomes something called the full system, thus the marginal in Bayes formula should integrate to 1? Maybe not .. It actually sums up to whatever the probability of the data is considering different parameters and their probabilities.
~~Sealt tuli mul mõte, et äkki isegi marginal integreerub üheks lähtudes, et $$P(data)=\int_{-\infty}^{\infty} P(data|\theta)P(\theta)=1$$ Aga see selleks . Bayesi statistikas on ta lõpuks ikkagi konstant. Continuing with it tomorrow.~~ 
https://stats.stackexchange.com/questions/58564/help-me-understand-bayesian-prior-and-posterior-distributions

+ **I dont think $$P(data)=\sum{P(data|\theta)P(\theta)}$$ will be larger than 1. It will still be probability. It wont=1 either this was my mixup point**


### CONJUGATE PRIOR AND POSTERITOR. FUNCTION FAMILYS, <- EXPONENTIAL FAMILY 

+ Conjugacy -  also video from oxbridge. 

### BETA DISTRIBUTION - DIRICHLET DISTRIBUTION


### RECAP OF **/Bayes Materials Bayes Method/Bayesgothrouhgh.pdf**

+ Here we use an example of 2 URNS with red and green balls: A & B.  1/3 of balls in A are red and 2/3 of balls in B are red. 
+ We chose a random urn not knowing which it is. Now we want to take a sample of size N(the data, which we will call X) from the urn get a certain belief about which urn we took. E.g. we want to get $$P(A|X),P(B|X)=P(\neg A|X)$$.   

**Initially our beliefs (Prior model) of taking urn A or taking urn B are both 1/2:**   
+ $$P(A)=P(B)=\frac{1}{2}$$.  


**Lets calculate the likelihood of getting X based on $$A$$ and $$\neg A$$. Lets say we took k red balls in the sample of size n. Then likeihoods for for $$A$$ adn $$\neg A$$ will be:** 

+ $$P(X|A)=(\frac{1}{3})^{k}(\frac{2}{3})^{n-k}$$ 
+ $$P(X|\neg A)=(\frac{2}{3})^{k}(\frac{1}{3})^{n-k}$$ 


The formula for likelihood - the *MODEL* mentioned previously comes from Bernoulli distribution $$B(p)$$ where p is the probability. In this case p is considered the parameter of the bernoulli distribution. We have 2 p-s. One p=1/3 probability of getting red ball from A and p=2/3 probability of getting red ball from B. p is considered the parameter in the bayesian inference here. 

**The overall probability for getting the data according to Bernoulli model(margin) is just goint to be a sum over all the parameters $$p$$.** 

$$P(X|A)=(\frac{1}{3})^{k}(\frac{2}{3})^{n-k} \frac{1}{2}$$ + $$P(X|\neg A)=(\frac{2}{3})^{k}(\frac{1}{3})^{n-k} \frac{1}{2}$$ 


**Now we have all the variables for using the Bayesian formula to get the Posterior model $$P(A|X)$$ - (Our belief into all the parameters after seeing the data)**


_Reminder of the general shape of the formula for bayesian inference_

$$\stackrel{\text{Posterior}}{P(\theta|data)}=\frac{\stackrel{\text{Likelihood}}{P(data|\theta)} \stackrel{\text{Prior}}{P(\theta)}}{\stackrel{\text{Marginal}}{P(data)} }$$


_Calculating the posterior_

+ $$P(A|X)=\frac{(\frac{1}{3})^{k}(\frac{2}{3})^{n-k}\frac{1}{2}}{(\frac{1}{3})^{k}(\frac{2}{3})^{n-k}\frac{1}{2} + (\frac{2}{3})^{k}(\frac{1}{3})^{n-k}\frac{1}{2}}$$

+ $$P(B|X)=\frac{(\frac{2}{3})^{k}(\frac{1}{3})^{n-k}\frac{1}{2}}{(\frac{1}{3})^{k}(\frac{2}{3})^{n-k}\frac{1}{2} + (\frac{2}{3})^{k}(\frac{1}{3})^{n-k}\frac{1}{2}}$$


For example, suppose that in n = 10 trials we got k = 4 red balls. The posterior probabilities would become $$P(A|X)=\frac{4}{5}$$ and $$P(B|X) = \frac{1}{5}$$


#### Generalisation of the previous

What Bayesian statistics does is replace this concept of likelihood by a real probability. In order to do that, we'll treat the parameter $$\theta$$ as a random variable rather than an unknown constant.This random variable $$\theta$$ itself has a probability mass function **which integrates to 1**: 
$$f(\theta)=P(\theta)$$This $$f(\theta)$$ is called the prior distribution on $$\theta$$.



BETA DISTRIBUTION 

_Here I did a short detour to youtube_
+ [Likelihood not probability](https://www.youtube.com/watch?v=sm60vapz2jQ&index=25&list=PLFDbGp5YzjqXQ4oE4w9GVWdiokWB9gEpm) Does not in itself integrate to 1 if we vary the theta.  There is no  rewquirement. 

+ [Order invariance](https://www.youtube.com/watch?v=_kmYpqvkKv0&index=27&list=PLFDbGp5YzjqXQ4oE4w9GVWdiokWB9gEpm)

![](order-invariance.png)

+ [Conjugate Prior Definition](https://www.youtube.com/watch?v=r0tRgR74n_g&index=28&list=PLFDbGp5YzjqXQ4oE4w9GVWdiokWB9gEpm) 
CONJUGACY:  If we choose prior and likelihood with some special distribution, it will lead to to a certain distribution.
+ [Conjugate prior Beta distribution](https://www.youtube.com/watch?v=v1uUgTcInQk&index=30&list=PLFDbGp5YzjqXQ4oE4w9GVWdiokWB9gEpm)  

+ [Proof Beta distribution conjugatu bernoulli,binomial](https://www.youtube.com/watch?v=hKYvZF9wXkk&index=31&list=PLFDbGp5YzjqXQ4oE4w9GVWdiokWB9gEpm)

**Beta Distribution** 

$$P(\theta|a,b)=\frac{\theta^{a-1}(1-\theta)^{b-1}}{B(a,b)}$$


+ [IMPORTANT EXAMPLE OF APPLICATION OF BAYESIAN INFERENCE AND BETA](https://www.youtube.com/watch?v=_nhn54l5dis&list=PLFDbGp5YzjqXQ4oE4w9GVWdiokWB9gEpm&index=35)'


Basically Beta distribution allows us to perform posterior inference. Thamt means get posterior DISTRIBUTION from prior and the data through a simple modification of the formula. Example of this: 

+ Data (X) has binomial distribution. We performed N=10 experiments, and X=1
+ $$\theta$$ has beta distribution
+ Initial Beta distribution is $$B(1,1)$$ After inference its going to be with distribution $$B(1+1,1+10-1)$$


![](bayes-beta1.png)
![](bayes-beta2.png)

**Additional info**

+ Prior gets more important as the size on N increases. 
+ The bigger the size of experiment x, the larger the effect on it on the prior. 
+ The posterior is somewhat a mean between the prior distribution and the data distribution. Both priors and data probabilities effect depends on the size of the data they are based. 
+ This videolist also explains, normal distribution, poisson distribution and gamma distribution. 





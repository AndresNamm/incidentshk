---
layout: "post"
title: "Linear Regression"
category: machine-learning

---



<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>



{% include toc %}
+ TOC
{:toc}  

# CHEAT SHEET OF VECTORIZED REGRESSION FORMULAS WITH GRADIENT DESCENT  


DEFINITIONS
+ $$X = m$$X$$n$$ m observations training data
+ $$y = m$$X$$1$$ m labels of training data
+ $$\theta = n$$X$$1$$ n parameters for training data
+ $$D=n$$X$$1$$ n parameter gradients for training data
+ $$\alpha= 1$$X$$1$$  learning rate
+ $$\lambda= 1$$X$$1$$ regularization term


##LINEAR REGRESSION WITH GRADIENT DESCENT

NOTES

+ You can perform Polynomial regression as well with x. Im not so sure about $$\theta$$ - in some cases it still might be convex.
COST FUNCTION

$$J(\theta)=\frac{1}{2m}\sum_{1}^{m}[(X\theta-y)^2]$$  

GRADIENT

$$\frac{\partial J(\theta)}{\partial \theta}=D=\frac{1}{m}(X'(X\theta-y))$$

GRADIENT DESCENT  

$$\theta=\theta-\alpha*\frac{\partial J(\theta)}{\partial \theta}$$

REGULARISATION

+ can be separately added

$$\text{cost function regularization term}=\frac{\lambda}{2m}\sum_{2}^{n}\theta^2$$

$$\text{cost function regularization term derivative}=\frac{\lambda}{m}\theta$$

## LOGISTIC REGRESSION

SIGMOID


$$sigmoid(x)=\frac{1}{1+e^{-X\theta}}=\frac{1}{1+e^{-h(\theta)}}$$

+ $$\lim_{h(x)->\infty} sigmoid(x)=1$$  
+ $$\lim_{h(x)->-\infty} sigmoid(x)=0$$  
+ $$h(x)=0=>sigmoid(x)=0$$

COST FUNCTION

$$J(\theta)=\frac{1}{2m}\sum_{1}^{m}-[y .* (ln(sigmoid(X\theta))+(1-y).* ln(1-sigmoid(X\theta))]$$  

** _$$ln(0)=\infty$$_  
** _$$ln(1)=0$$_  

GRADIENT   

Same as in linear regression



GRADIENT DESCENT  


Same as in linear regression  

REGULARISATION

Same as in linear regression  

## CALCULATING THETA ANALYTICALLY - NORMAL EQUATION

NOTES

+ Can not be used for logistic regression. Logistic regression does not form a linear equation.  


METHOD   

1. Set partial derivative matrix of cost function to 0

$$\frac{\partial J(\theta)}{\partial \theta}=0$$  

2. Solve the linear equation for $$\theta$$ through this derived equation.

$$\theta = (X^T X)^{-1}X^T y$$

REQULARIZATION


$$$$
\begin{align*}& \theta = \left( X^TX + \lambda \cdot L \right)^{-1} X^Ty \newline& \text{where}\ \ L = \begin{bmatrix} 0 & & & & \newline & 1 & & & \newline & & 1 & & \newline & & & \ddots & \newline & & & & 1 \newline\end{bmatrix}\end{align*}
$$$$

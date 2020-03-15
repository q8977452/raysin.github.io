---
layout: post
title:  "Approximation of Function"
date:   2020-03-15 22:32:30 +0800
categories: Numerical_Analysis
tags : Fouerier Chebyshev Approximation Series
mathjax: true
---
## **Chebyshev Polynomials and Chebyshev Series**
*  Chebyshev Polynomials
	
	* Chebyshev's differential equation:

		![](http://latex.codecogs.com/gif.latex?(1-x^2)y^{"}-xy^{'}+\lambda%20y=0%20,where%20-1%20\leq%20x%20\leq%201)

	* Two-term recursion formula:

		![](http://latex.codecogs.com/gif.latex?T_{n+1}(x)=2xT_n(x)-T_{n-1}(x)%20\quad%20with%20\quad%20T_0(x)=1,\quad%20T_1(x)=x)

	* An orthogonal set:

		![](http://latex.codecogs.com/gif.latex?\int_{-1}^1\frac{1}{\sqrt{1-x^2}}T_n(x)T_m(x)dx=)
							
		1. ![](http://latex.codecogs.com/gif.latex?0,%20n%20\neq%20m)
				
		2. ![](http://latex.codecogs.com/gif.latex?\pi,%20n=m=0)
				
		3. ![](http://latex.codecogs.com/gif.latex?\frac{\pi}{2},%20n=m%20\neq%200)
	
* Smallest Upper Bound / Smallest Maximum Error (最小的最大誤差)
	
	![](http://latex.codecogs.com/gif.latex?\frac{1}{2^{n-1}}T_n(x))
	
	* 證明可以用矛盾法(contradiction)去證
	
* Chebyshev Series
	
	The computational economy to be gained by economizing a Maclaurin series or 
	by using a Chebyshev series is even more dramatic when the Maclaurin series is slowly convergent.
	(當 Maclaurin 級數收斂緩慢時，使用 econimized 級數或 Chebyshev 級數來「降低最大誤差」或「將誤差平均化」的效果會更明顯)
	
	優點:
	
	* at significant savings of computational effor
		
	* withsmallerstoragerequirementsinacomputer’smemoryforthecoefficients of the polynomials
	

## Rational Function Approximations (有理式函數之近似)

* Approximating a known function with a Chebyshev series is much better than with a Taylor series in that it has a smaller maximum error in the interval [-1, 1]. However, thereis still a way to improve further.

* **Pade Approximations**
	
	![](http://latex.codecogs.com/gif.latex?\frac{a_0+a_1x+a_2x^2+\dots%20+a_nx^n}{b_0+b_1x+b_2x^2+\dots%20+b_mx^m},%20N=n+m)




## Fourier Series (傅立葉級數)

	Polynomials are not the only functions that can be used to approximate known functions. 
	Another means for representing known functions are approximations that use sines and cosines, 
	called Fourier series after the French mathematician who first proposed, in the early 1800s,
	that“any function can be represented by an infinite sum of sine and cosine terms.”

	Representing a function as a trigonometric series is important in 
	solving some partial differential equations analytically.

* **Fourier Series**
	
	* Standard Form
	
		![](http://latex.codecogs.com/gif.latex?\frac{1}{2}a_0+\sum_{n=1}^{\infty}[a_ncos(\frac{n\pi%20x}{L})+b_nsin(\frac{n\pi%20x}{L})])

		1. ![](http://latex.codecogs.com/gif.latex?a_0=\frac{1}{L}\int_{-L}^{L}f(x)dx)
		
		2. ![](http://latex.codecogs.com/gif.latex?a_n=\frac{1}{L}\int_{-L}^{L}f(x)cos(\frac{n\pi%20x}{L})dx\quad%20for\quad%20n=1,2,3,...)
		
		3. ![](http://latex.codecogs.com/gif.latex?b_n=\frac{1}{L}\int_{-L}^{L}f(x)sin(\frac{n\pi%20x}{L})dx\quad%20for\quad%20n=1,2,3,...)
	
	* Phase Angle Form
	
		![](http://latex.codecogs.com/gif.latex?\frac{1}{2}a_0+\sum_{n=1}^{\infty}c_ncos(\frac{n\pi%20x}{L}+\delta_n))
		
		1. ![](http://latex.codecogs.com/gif.latex?c_n=\sqrt{a_n^2+b_n^2})
		
		2. ![](http://latex.codecogs.com/gif.latex?\delta_n=-tan^{-1}(\frac{b_n}{a_n}))

	* Complex Form
	
		![](http://latex.codecogs.com/gif.latex?\sum_{-\infty}^{\infty}d_ne^{in\omega_0x})
		
		1. ![](http://latex.codecogs.com/gif.latex?d_n=\int_{-L}^{L}f(x)e^{in\omega_0x}dx)
		
		2. ![](http://latex.codecogs.com/gif.latex?\omega_0=\frac{\pi}{L})
	
	
* **The Gibbs Phenomenon (Gibbs 現象)**
		
	The behavior of Fourier series at jump discontinuities of the function is known as the Gibbs phenomenon.
	It means that the peaks in neighbor of the jump discontinuities do not become smaller as N -> ∞. 
	Instead, the peaks maintain roughly the same height, but move closer to the discontinuities as N increases.
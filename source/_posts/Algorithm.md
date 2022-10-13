---
title: Algorithm
date: 2022-06-28 19:26:00
tags: algorithm notes
---

# Algorithm

### 1dim peak finder

$$\Theta (n)  $$ 

if we adopt **divide & conquer** 

the complexity will be 

$$T(n) = T(\frac n2)+ \Theta(1)$$

and there is the base case :

$$T(1)=\Theta(1)$$

thus 

$$
T(n)= \Theta(1) +\Theta(1)+...+\Theta(1)
	= \Theta(\log _{2} n)
$$


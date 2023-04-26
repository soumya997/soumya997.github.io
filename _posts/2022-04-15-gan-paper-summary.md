---
layout: post
title: GAN - Generative Adversarial Nets Paper Summary and Notes
subtitle: Over view of GAN, need of GAN, workings, Objective function, derivations. 
cover-img: /assets/img/celabA.png
thumbnail-img: https://user-images.githubusercontent.com/54326088/221641237-f821efd6-eb2b-4ea2-b2cd-4d847fbd96fa.gif
share-img: https://user-images.githubusercontent.com/54326088/221641237-f821efd6-eb2b-4ea2-b2cd-4d847fbd96fa.gif
tags: [Deep Learning, Paper summary, note]
---


<!-- ## GAN: Generative Adversarial Nets Paper Review and Notes: -->

<p align="center">
<img src="https://miro.medium.com/max/1400/1*StXrVTHgomba3jBlNhn_mw.png">
</p>



We know How GAN works, right?, GAN = "generative adversarial network", it has two parts generative and Adversarial part. 
The main objective of GAN is to mimic a distribution, or we can say we use GAN for distribution modeling.

We know what are the parameters we need to define a normal distribution or uniform distribution. Now, given a random distribution how do we know the distribution parameters, so to get the distribution parameters we use GAN.
eg, 
- You have height data of each individual student of a class. And given that you want to generate similar heights that match the data distribution. 
- In terms of images one example would be, given a dataset consisting of handwritten digits, you want to generate similar handwritten images but not ditto copy of the images from the dataset.


### Need of GAN:
You might think why do we need GAN, when we can find out the distribution of the data from some statistical model or some thing. And you queation is totally valid, lets take the height example. Here we might have just plotted the data, and see what kind of distribution that is, from the plot we could find out if that is normal or uniform or beta or poisson or so on and test that via some hypothesis testing like QQ plot or KS-test etc. And when we know the distribution we will use the formulas to calculate the distribution params of that respective distribution.
say, the distribution of the heights was a normal distribution, so, $\text {distribution params}  P_\theta (H) = N[\theta = {\mu,\sigma} = 190,40]$. And based on this params we can now generate new samples. and to check how similar the generated and real sample is we can use some distance measurement methods like KL-Divergence or JK-Divergence etc. based on this we can further improve the distribution params and get to the desired o/p. 
But this height example was using 1D data, in case of images of size 28x28, it will be 784D data. And these statistical methods actually dont work that efficiently in case of multidimensional data. Because of that we need to relay on GAN or neural net based approaches. Not only GANs but Autoencoders also does the same thing using ANNs.

### Let's discuss the few paper highlights of GAN,
- Given a data distribution $P_x$ , we need to model a generator function $G$  that can approximate $P_x$ . 
- For that, we take $z$ from a normal/uniform distribution denoted as $P_z$, and pass that through $G$ for training. $G$ generates a sample $G(z)$, simultaneously we sample a data point from $P_x$, say $x$. We pass that through a discriminator function/model $D$. 
- This $D$ tries to distinguish b/w the real($x$) and the fake($G(x) = \hat{x}$) smaple. 


<p align="center">
<img src="https://i.imgur.com/l44tYie.png">
</p>


- We calculate a join[for D and G] loss function, and update the gredients through backpropagation. The objective/loss function looks like this,
> $\min\limits_{G} \max\limits_{D} V(D,G) = E_{x \sim P_{data}(x)} [\log{D(x)}] + E_{z \sim P_{Z}(z)} [\log{(1 - D(G(z)))}]  ...(1)$
- Now, $E_x \sim P_{data}(x)  [\log{D(x)}]$ is the probability of real data being classified as real data and 
$E_x \sim P_{Z}(x)  [\log{(1 - D(G(z)))}]$ is the probability of fake data being classified as fake data. And we want to increase both of them. If probability of fake data being classified as real is $D(G(z))$ then $1 - D(G(z))$ will represent the fake data being classified as fake.  
- Early in learning, when $G$ is poor, $D$ can reject samples with high confidence because they are clearly different from the training data, 
    - by that i mean $\log{(1 - D(G(z)))}$ this term saturates.
    - (D can reject generated samples with high confidence)
    - $D(G(x)) \approx 0$ and $1 - D(G(x)) \approx 1$ and  $log(1 - D(G(x))) \approx 0$ 
    - And the solution is, instade of minimizing the $1 - D(G(x))$ wrt generator [probab of the fake classified as fake] we maximize the $D(G(x))$ wrt generator [probab of fake classified as real]
    - 

<p align="center">
  <img src="https://i.imgur.com/KT1v6lG.png">
</p>

- $D(x)$ is probability of x being classified as real/true and $1-D(X)$ is probability of x being classified as fake/false.

- $D(x)$ = probability of real being classified as real
- $D(G(x))$ = probability of fake being classified as real
- $1 - D(G(x))$ = probability of fake being classified as fake

The descriminator tries to increase D(x) and 1 - D(G(x)), which means the descriminator tries to detect real as real and fake as fake.

On the other hand the generator tries to minimize $1-D(G(x))$, which means it tries to maximize D(G(x)), that means tries to fool the descriminator by increasing the probability of detecting fake/generated images as real. 

### proofe of optimal D is Max of Eqn 1:
They give a proposition in the paper, which is,
- For fixed G, the optimal discriminator $D^*$ is
    - $D_G^*(x) = \frac{P_{data}(x)} {P_{data}(x) + P_g(x)}  ... (k)$  


#### **Proofe:**
We know the training criterion for the descriminator $D$, given any generator $G$ is to maximize eqn 1.
      - Denotes  $D_G^*(x) = argmax_{D}  V(D,G)$   
      - And please note,
            - $E_{p(x)}[x] = \int\limits_{x} x p_x(x) dx$ , $E_{p(x)}[x]$ is Expectation of a random variable $x$, having probability density function $p(x)$.

Now, 
$argmax_{D}  V(D,G) = argmax_{D}  \left[  E_{x \sim P_{data}(x)}  [\log{D(x)}] + E_{z \sim P_{Z}(z)}  [\log{(1 - D(G(z)))}])  \right]$
But in the paper, it is given as,
 
<p align="center">
<img src="https://i.imgur.com/Xao2Yqr.jpg">
</p>

please note the changes, in the 1st line it is $p_z(z)$ and the integration is happening wrt $z$. But in 2nd line, $P_z(z)$ is being replaced with $P_g(z)$ and the integration happening wrt $x$. 

Actually we can replace those two terms according to the rule of probability density function, which is

> If the probability density function of a random variable $x$ is defined as $P_X(x)$,then it is possible to calculate the PDF of some variable $y = G(x)$, which is a function of $x$.
> This is called "Change of Varibale" and it is defined as 
> 
> $P_Y(y) = P_x(G^{-1}(y))  \left[\frac{d} {dy} (G^{-1}(y))\right]$ 
> 
> <p align="center">
    <img src="https://i.imgur.com/fMlHEwy.png">
    </p>
> 

- In our case, it is,
    - $P_g(x) = P_z(G^{-1}(x))  \left[\frac{d} {dx} (G^{-1}(x))\right]  ...  (a)$ 
    - <p align="center">
      <img src="https://i.imgur.com/4H6483h.png">
      </p>


- We know $z = G^{-1}(x)$ and $G(z) = x$, and we have this part of the eqn to deal with,
    - $\int\limits_{z}(z)  log(1 - D(G(z)))  dz  ...(2)$
- So we replace z in eqn 2, and we get,
    - $\int\limits_{x}( G^{-1}(x))  log(1 - D(x))  d G^{-1}(x)  ...(3)$ 
    - multiply $dx$ in both numorator and denominator,
    - $\Rightarrow \int\limits_{x}( G^{-1}(x))  log(1 - D(x))  \frac {d G^{-1}(x)} {dx} dx$ 
    - $\Rightarrow \int\limits_{x} {P_z(G^{-1}(x))  \left[\frac{d} {dx} (G^{-1}(x))\right]}  \times  log(1 - D(x)) dx$ 
    - Now we can replace the part before the multiplication sign with eqn a.
    - $\Rightarrow \int\limits_{x} P_g(x)  \times  log(1 - D(x)) dx$ 
- optimal descriminator $D^*$ for a given G is obtained by maximizing $V(D,G)$. And the maximization of V can be done by taking its 1st order derivative wrt D(x).
    - So, $\frac{d}{dD(x)}\left[P_{data}(x)  \log{D(x)} + P_g(x)  \log(1 - D(x))\right] = 0$
    - $\Rightarrow \frac{P_data(x)}{D(x)} - \frac{P_g(x)}{1 - D(x)} = 0$
    - $\Rightarrow D_G^*(x) = \frac{P_{data}(x)} {P_{data}(x) + P_g(x)}$  (Proved)
- Now this proves the eqn $k$.

### Proofe of JSD from the V(D,G):
<p align="center">
<img src="https://i.imgur.com/oTvLg6b.png">
</p>

<p align="center">
<img src="https://i.imgur.com/aEmVlhU.png">
</p>

In the 2nd line we replace $D(x)$ with $D^{\ast}(x)$, and put the value of $D^{\ast}(x)$.


### How the training of $G$ and $D$ happens:
Read the algorithm correctly. First we fix the $G$ and update the $D$ params then we fix $D$ and update the $G$ params.
<p align="center">
<img src="https://i.imgur.com/WzDxOO5.png">
</p>
### Few terms that are used in literatures:
1. **Prior:** This is the noise vector that we give input to the generator. It is generally of size 100x1 we can write it as $z \in \mathbb{R}^{100}$. We sample that vector from a normal distribution. And over time as the training goes on, the 100-dimensional image gets transformed into 784 dimension vector(I'm talking bout MNIST). As the progress kept happening in the field, ppl found different ways to manipulate that noise vector to get desired generated images and one of the works that came out showcasing the manipulation of the prior data for getting the desired o/p is info-GAN.
2. **Multi-modal prior and Uni-model prior:** when you use a single 100x1 noise vector then it is called a Uni-model prior. but more 100x1 vector, input to the generator is called Multi-model prior.
3. **what is $z$ $\sim$ $P_z$ :** The "$\sim$" symbol represents, sampled from. Here $z$ is smapled from $P_z$ . $P_z$ is the distribution of z.
4. **Difference b/t $P_z$ and $P_Z(z)$ :** $P_z$ is the distribution from where $z$ is sampled, $P_z$ is also considered as $Z$. This can also be written interms of probability distribution, $P(z)$. The subscript $Z$ of $P_Z(z)$ defines the total distribution. 
5. $E_{z \sim P_z(z)}  [\log(D(x))]$  The $E$ is called, Expectation. The defination of $E$ is provided in the 1st proofe.


### References:
- https://arxiv.org/abs/1406.2661

---
layout: post
title: Info-GAN- paper summary and Notes
subtitle: Over view of Info-GAN, Need of info-GAN, workings, Objective function, derivations. 
cover-img: /assets/img/header_mnist.png
thumbnail-img: https://i.imgur.com/g8QElwL.png
share-img: https://i.imgur.com/g8QElwL.png
tags: [Deep Learning, Paper summary, note]
---


<!-- ## GAN: Generative Adversarial Nets Paper Review and Notes: -->

<p align="center">
<img src="https://i.imgur.com/TeZe344.png">
</p>



## Important Notes: 

- Only when you are interpolating you will find meaning in $z$. By interpolation I mean the, arthmatic operation performed on averaged $z$ to change gender of a person waring glasses[man waring glasses -> woman waring glasses]. But when you want to associate meaning in $z$ there is no meaning its just a random vector.
- The latent vector[prior] $z$ is highly entangled and unstructured, it means we dont know what points in that vectors contatints the specific representation that we want. But we can introduce meaning in those $z$ vectors. To prove that Info-GAN comes to the picture. 
- How introducing meaning in $z$ can help? By doing that you will make $z$ more interpretable. By that I mean, you will know how to adjust $z$ in order to get a desired output.
- So the paper, info-GAN is all about introducing meaning in the latent variables/codes (c1,c2..cn).  


<p align="center">
<img src="https://i.imgur.com/oiDGrAe.png">
</p>

here you can see, that varying c1 in info-GAN will generate a consistent numbers in a row(same numbers in the row, although it has been sorted by the authors). But varying c1 in regular GAN will generate inconsistent results. In the image (c) you can see that varying $c_2$ in info-GAN targets the rotation of an digit, and in image (d) $c_3$ impacts the thickness/width of the digits. These properties of the dataset is called disentangled features/representations. For example:

> Disentangled Representations,
> - hair styles, presence/absence of eyeglasses and emotions on **CelebA**
> - background digits from the central digit on **SVHN**
> - pose from lighting of 3D rendered images
> - writing styles from digit shapes on **MNIST**

- remember wheneven you read about disentanglment in the GAN literature, it is mainly has something to do with manipulation of the generated images by changing different properties of that image, eg: changing the width/rotation of the handwritten digits.


## Info-GAN Framework:
- in info-GAN we provide two vectors [concatenated], 1) latent vector/prior, 2) latent code($c_1$,$c_2$ etc). Now the generator model $G$ is a function of $z$ and $c$, represented as $G(z,c)$.
- These latent codes are two types, discreate and continuous. From the above picture, $c_1$ is a categorical code.


<p align="center">
<img src="https://i.imgur.com/L8yg2ir.png">
</p>

- $c_2, c_3$ are continuous codes which continuous entangalments.

<p align="center">
<img src="https://i.imgur.com/oBy4WqU.png">
</p>

- In the title we see information maximization GAN, but maximizing information b/t what? So, we try to maximize the information b/t $c$ and $G(z,c)$. Thats why we add the term $I(c;G(z,c))$in the objective function, $I$ is mutual information. The responsibility of modifying $c$ to increase the mutual info lies upon the $Q$ network. which is a part of discriminator network $D$, just with an aditional fully connected layer, this way it only adds a negligible computation cost to [GAN](https://soumya997.github.io/2022-04-15-gan-paper-summary/). 

- You will find both representation of the info-GAN framework, but both are same, $Q$ uses $D$, just with a change of a extra fully connected layer.

| Q as a individual network | Q as a part of D |
|-------|--------|
| <img src="https://i.imgur.com/g8QElwL.png"> | <img src="https://miro.medium.com/max/1086/1*c0wSI0WJR9-yagc0ruFGGg.png">  |

## Objective function:
Below is the objective function, separated in two parts, regular GAN loss + regularization [info-GAN loss].

$\min_{G} \max_{D} V_{I}(D, G)=V(D, G)- \lambda I(c ; G(z, c))$ , $\lambda < 0$  (mentioned in paper)

> [GAN loss] $\min\limits_{G} \max\limits_{D} V(D,G) = E_{x \sim P_{data}(x)}  [\log{D(x)}] + E_{z \sim P_{Z}(z)}  [\log{(1 - D(G(z)))}]$ 



The 2nd part helps to increase the mutual information. Looking at the objective function you can tell that, training objective is to maximize the $D$ and minimize the $G$. 

> maximize $D$, $\max_{D} V_{I}(D, G) = \max_{D} E_{x \sim P_{data}(x)}  [log(D(x))]$

and,

> minimize $G$, $\min_{G} V_{I}(D, G)$ = $\min_{G} E_{z \sim P_{Z}(z)}  [\log{(1 - D(G(z)))}] -\lambda I(c ; G(z, c))$ 
> [coz these two terms includes generator part]


Here, to minimize the generator loss we need to increase $\lambda I(c ; G(z, c))$, on the other hand mutual information is defined as 

> $I(X ; Y)=H(X)-H(X \mid Y)=H(Y)-H(Y \mid X)$ ...(a), Here $X = c$ and $Y = G(z,c)$

so to increase $\lambda I(c ; G(z, c))$ we need to minimize $H(X \mid Y)$.
And when $H(X \mid Y) \to 0$ or $H(X \mid Y) = 0$, from information theory we say that, $X$ can be completely determined from $Y$. So now we can say, c can be completely determined from G(z,c). This is the base concept of **info-GAN**. 

<br>
To optimize the mutual information $I$, we use the concept of Variational Mutual Information Maximization. which says, I is hard to maximize directly as it requires the access to the posterior $P(c|x)$, here $x$ implies $\hat{x}$. But we can get the lower bound of I, by defining an auxiliary distribution $Q(c|x)$. 
<br>

By doing the calculations for $I$ by using equestion (a), you will eventually reach the position where, $I = H(c) + KLD(p,Q) +$ $E_{c \sim P(c), x \sim G(z, c)}[\log Q(c \mid x)]$ 
 And as we know kld of two distribution is always $\geq 1$ , so using that we can write, $I \geq H(c) + E_{c \sim P(c), x \sim G(z, c)}[\log Q(c \mid x)]$. This is the lower bound.

<img width="500" src="https://i.imgur.com/blQpcDt.png">

 From there we get,
 > $\min_{G, Q} \max_{D} V_{InfoGAN }(D, G, Q) = V(D, G) -\lambda L_{I}(G, Q)$

This is the main objective function of Info-GAN.

## Questions that this Paper Answers:
- ```NotImplementedError```
## Quality Check:
- ```NotImplementedError```
## References:
- ```NotImplementedError```

























---
layout: post
title: Elastic Info-GAN- Paper Summary
subtitle: Motives/problems of Elastic info-GAN, solution of those problems. 
cover-img: /assets/img/elastic-infogan.png
thumbnail-img: https://i.imgur.com/4VajmgQ.png
share-img: https://i.imgur.com/4VajmgQ.png
tags: [Deep Learning, Paper summary, note]
---


<!-- ## GAN: Generative Adversarial Nets Paper Review and Notes: -->

<p align="center">
<img src="https://i.imgur.com/JyZdw24.png">
</p>



### Motive of the Paper:
This paper tries to exploit mainly two faults of the Info-GAN paper, by keeping the other good qualities/improvements intact. These two shortcomings are, 
- We have gone through [Info-GAN](https://soumya997.github.io/2022-04-16-infogan-paper-summary/), where it mainly focuses on generating disentangled representation by manipulating the latent code vector $c$ [please go through the info-GAN blog if the terms looks ambiguous to you]. In that we have seen that it considers that, this latent code vector is made of continuous and discrete latent variables. Now, one of the assumption they used were, the discrete latent variable has uniform distribution -> $c_1 \sim Cat(K=10,p=0.1)$ [for MNIST]. That implies that the class distribution they are providing are balanced. But reallife dataset might not have a balanced categorical distribution. Imbalance data will try to fource the generator to generate more images that belongs to the majority classes. 


<p align="center">
<img src="https://i.imgur.com/3hSRrzr.png">
</p>

- Although info-GAN generates quality images when provided consistent class distribution,but it faces trouble generating consistent images from the same category for a latent dimension given a imbalanced dataset. See the center image row 1,2,4,7 etc. Their reasoning for that is, there are other low-level factors (e.g., rotation, thickness) which the model focuses on while categorizing the images.


## solutions to those flaws:
#### Solution of the 1st problem:
They remodeled the way the latent distribution is used to fetch the latent variables; they lift the assumption of any knowledge about the underlying class distribution, where instead of deciding and fixing them beforehand, 

They treat the class probabilities as learnable parameters of the optimization process. To enable the flow of gradients back to the class probabilities, they employ the Gumbel-Softmax distribution. replace the fixed categorical distribution in InfoGAN with the Gumbel-Softmax distribution, which enables sampling of differentiable samples.

Gumbel-Softmax is a continuous distribution, I will discuss behind the reasoning of using a continuous distribution for modeling a categorical distribution later. But this continuous distribution can be annealed into a categorical distribution. Say, $p_1,p_2,p_3,...,p_k$ are the class probabilities $p_i$, where $k$ number of classes. Now, we can create a k-dimentional vector $c$, whose every element can be given by,

>$\LARGE c_{i}=\frac{\exp \left(\left(\log \left(p_{i}\right)+g_{i}\right) / \tau\right)}{\sum_{j=1}^{k} \exp \left(\left(\log \left(p_{j}\right)+g_{j}\right) / \tau\right)}$  [here, i= 1,2,...k]
> 
> Here $g_i, g_j$ are samples drawn from $Gumbel(0, 1)$, and $\tau$ (softmax temperature) controls the degree to which samples from Gumbel-Softmax resemble the categorical distribution. Low values of $\tau$ make the samples possess properties close to that of a one-hot sample. $p_i$ are class probabilities and $p_{j}=0(\forall j \neq i)$.

> InfoGAN’s behavior in the class balanced setting (Fig. 1 left) can be replicated in the imbalanced case (where grouping becomes incoherent, Fig. 1 center), by simply replacing the fixed uniform categorical distribution with Gumbel-Softmax with learnable class probabilities $p_i$’s; i.e. gradients can flow back to update the class probabilities (which are uniformly initialized) to match the true class imbalance. 


#### Solution of the 2nd problem:

<p align="center">
<img src="https://i.imgur.com/JlTQXQr.png">
</p>

To solve the 2nd problem they enforce $Q$ to learn representations using a contrastive loss. The idea is to create positive pairs (e.g., a car and it’s mirror flipped version) and negative pairs (e.g., a red hatchback and a white sedan) based on object identity, and have $Q$ produce similar and dissimilar representations for them respectively. See the above image to correlate. Formally, for a sampled batch of $N$ real images, we construct their augmented version, by applying identity preserving transformations ($\delta$) to each image, resulting in a total of $2N$ images. For each image $I_i$ in the batch, we define the corresponding transformed image as $I_{pos}$, and all other $2(N− 1)$ images as $I_{neg}$.

Here loss has been calculated for each images, say loss for each image is, $l_i$, and the total loss would be $\large L_{ntxent} = \sum\limits_{i=1}^{N}l_i$ .
Now, $l_i$ is defined as,

$\Large \ell_{i}=-\log \frac{\exp \left(\operatorname{sim}\left(f_{i}, f_{j}\right) / \tau^{\prime}\right)}{\sum_{k=1}^{2 N} \mathbf{1}_{[k \neq i]} \exp \left(\operatorname{sim}\left(f_{i}, f_{k}\right) / \tau^{\prime}\right)}$ ...(a)

where $j$ indexes the positive pair, $f$ represents the feature extracted using $Q$ (we use the penultimate layer), $\tau^{'}$ is a softmax temperature, and $sim(.)$ refers to cosine similarity.

This loss is added to the main info-GAN loss, after adding that part the whole loss look like this,

$\Large \min_{G, Q, p_{i}} \max_{D} L_{f i n a l}=V_{\text {InfoGAN }}\left(D, G, Q, p_{i}\right)+\lambda_{2} L_{n t x e n t}(Q)$

$V_{InfoGAN}$ plays the role of generating realistic images and associating the latent variables to correspond to some factor of variation in the data, while the addition of Lntxent will push the discovered factor of variation to be close to object identity.




















---
title: Variational Inference
date: 2019-02-23
math: true
menu:
  sidebar:
    name: Variational Inference
    identifier: variational_inference
    parent: machine_learning
    weight: 4
---

# Introduction

Variational inference is an important topic that is widely used in machine learning. For example, it's the basis for variational autoencoders. Also Bayesian learning often makes use variational of inference. To understand what variational inference is, how it works and why it's useful we will go through each point step by step.

## What are latent variables?

A latent variable is the opposite of an *observed* variable. This means that a latent variable is not directly observed but inferred from other variables which are observed. [This book](https://learnche.org/pid/contents) provides a nice conceptual example:
    
> **Example:**
Consider your overall health. There are multiple measurements we can use to assess health, were each measurement is looking at a certain physical property, for example blood pressure or body temperature. However, 'health' remains an abstract concept that cannot be measured directly. In this sense, health is a latent variable, whereas blood pressure and body temperature are observable variables.

## What is variational inference?

Variational inference is a machine learning method which allows us to approximate probability distributions. In many real world problems we are faced with probability distributions that can't be computed. This especially often happens when a distribution involves latent variables. Therefore, we need strategies to approximate such distributions. Variational inference is one method for doing this. Several other methods exist which broadly fall into two classes: methods that rely on *stochastic* approximations (like [Markov chain Monte Carlo sampling](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo)) and those that rely on deterministic approximations (like variational inference).

A key characteristic of variational inference is that it *reframes* the original problem (computing some probability distribution) into a simpler problem which can be solved. Different to this, Markov chain Monte Carlo sampling directly approximates the target distribution by sampling from it.

A high level example for variational inference is given [here](https://www.quora.com/What-is-variational-inference):   

> **Example:** Variational inference is similar to what often happens when attending a presentation or lecture. Someone in the audience asks the presenter a very difficult answer which she can't answer. Instead of answering the original, difficult question, the presenter reframes the question into an easier one which can be answered exactly.

## Problem set-up

Suppose we have the following set-up:
- A set of observations $ \pmb{x} = x_1, ..., x_n $
- A set of latent variables $\pmb{z} = z_1, ..., z_l$
- The joint probability distribution is given by $p(x, z)$
- In many probabilistic models we are interested in the posterior distribution $p(\pmb{z} \, | \, \pmb{x})$ of the latent variables given the observed data $\pmb{x}$:

$$ p(\pmb{z} \, | \, \pmb{x}) = \frac{p(\pmb{z},\pmb{x})}{p(\pmb{x})}$$

This posterior distribution can be used for several purposes. For example, it can be used to provide point estimates for the latent variables.


**Problem:** For many models it's impossible to evaluate this posterior distribution or even to compute expected values with respect to the distribution. To evaluate $p(\pmb{z} \, | \, \pmb{x})$ we need to compute the denominator $p(\pmb{x})$ which is called the *evidence*. The evidence is given by $p(\pmb{x}) = \int p(\pmb{z, x}) d\pmb{z}$. For many models this integral cannot be computed or takes exponential time to compute. For example, the dimensionality of the latent variables might be too high.

**Solution:** We approximate $p(\pmb{z} \,|\, \pmb{x})$ using variational inference.

## How does variational inference work? 

To approximate $p(\pmb{z} \,|\, \pmb{x})$ we introduce a *variational distribution* over the latent variables $q(\pmb{z})$. More precisely, we choose a *family of distributions* $\mathcal{Q}$ characterized by some parameters $\theta$. For example, we could decide that our variational distribution belongs to the family of Gaussian distributions. In this case the parameters $\theta$ would be the mean and standard deviation of the Gaussian distribution. Each member $q(\pmb{z}) \in \mathcal{Q}$ represents a candidate approximation to the true posterior $p(\pmb{z} \,|\, \pmb{x})$.

Our goal is to find the best candidate distribution. Or, to be more precise, to find the setting of the parameters $\theta$ that make our candidate $q(\pmb{z})$ as similar as possible to $p(\pmb{z} \,|\, \pmb{x})$.

Of course the choice of the variational family $\mathcal{Q}$ has a large impact on the final result. The true posterior is often not contained in the variational family. However, we don't need to find the exact posterior. We just want to find a (very) good estimate.

## KL divergence 

We evaluate our candidate variational distribution using the *Kullback-Leibler divergence* which was introduced in [this post](). The KL divergence can be used to measure how similar $q(\pmb{z})$ is to the target distribution $p(\pmb{z} \,|\, \pmb{x})$. To find the best variational distribution we minimize the KL divergence:

$$q_{\text{best}}(\pmb{z}) = \arg\min_{q(\pmb{z}) \in \mathcal{Q}} D_{KL}\big(q(\pmb{z}) \,||\, p(\pmb{z} \,|\, \pmb{x})\big)$$

The KL divergence is defined as:

$$D_{KL}\big(q(\pmb{z}) \,||\, p(\pmb{z} \,|\, \pmb{x})\big) = \int_{-\infty}^{\infty} q(\pmb{z}) \log\big( \frac{q(\pmb{z})}{p(\pmb{z} \,|\, \pmb{x})} \big) = \mathbb{E}_{q(\pmb{z})}\big[\log\big( \frac{q(\pmb{z})}{p(\pmb{z} \,|\, \pmb{x})} \big) \big]$$

**Problem:** The KL divergence can't be computed. To see why we reformulate the definition of the KL divergence:

$$ \begin{align}
\mathbb{E}\_{q(\pmb{z})}\big[\log\big( \frac{q(\pmb{z})}{p(\pmb{z} | \pmb{x})} \big) \big]
&= \mathbb{E}\_{q(\pmb{z})} \big[ \log \big( q(\pmb{z}) \big) \big]- \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{z} | \pmb{x}) \big) \big] \\\\
&= \mathbb{E}\_{q(\pmb{z})} \big[ \log \big( q(\pmb{z}) \big) \big]- \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{z},\pmb{x}) \big) \big] + \log \big( p(\pmb{x}) \big) 
\end{align} $$

The last term in this equation is exactly the evidence we came across earlier $p(\pmb{x}) = \int p(\pmb{z, x}) d\pmb{z}$ and which can't be computed.

**Solution:** Instead of minimizing the KL divergence, we minimize an alternative quanitity which is equivalent up to an added constant. This is the so called *evidence lower bound* or *ELBO*:

$$\text{ELBO}(q) = - \mathbb{E}\_{q(\pmb{z})} \big[ \log \big( q(\pmb{z}) \big) \big] \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{z},\pmb{x}) \big) \big]$$

When comparing the ELBO with the KL divergence we can see that the ELBO is simply the negative KL divergence plus our problematic evidence term $p(\pmb{x})$. Maximizing the ELBO is equivalent to minimizing the KL divergence. 

## Evidence lower bound  

To gain a deeper understanding of what it means to find the optimal candidate ${q(\pmb{z})}$ we can rewrite the evidence lower bound:

$$
\begin{align}
\text{ELBO}(q) &= - \mathbb{E}\_{q(\pmb{z})} \big[ \log \big( q(\pmb{z}) \big) \big]+ \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{z},\pmb{x}) \big) \big] \\\\
&= - \mathbb{E}\_{q(\pmb{z})} \big[ \log \big( q(\pmb{z}) \big) \big] + \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{z}) \big) \big] + \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{x} | \pmb{z}) \big) \big] \\\\
&= \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{x} | \pmb{z}) \big) \big] - D\_{KL}\big(q(\pmb{z}) || p(\pmb{z})\big)
\end{align}
$$

Let's take a closer look at the individual terms:

1. The first term $ \mathbb{E}_{q(\pmb{z})} \big[ \log \big(p(\pmb{x} | \pmb{z}) \big) \big]$ describes the probability of the data given the latent variables. By maximizing the ELBO, we encourage the optimization process to choose a candidate distribution $q(\pmb{z})$ which explain the observed data well.    
2. The second term $- D_{KL}\big(q(\pmb{z}) || p(\pmb{z})\big)$ is the negative KL divergence between our variational distribution $q(\pmb{z})$ and the prior distribution over the latent variables $p(\pmb{z})$. Maximizing this term corresponds to minimizing the KL divergence. So the optimization process is encouraged to make the variational distribution similar to the prior distribution over the latent variables.

## Why it's called evidence lower bound 

The name 'evidence lower bound' comes from an important property of the ELBO: it provides a lower bound on the (log) evidence $p(\pmb{x})$.

We already determined that:
1. $D\_{KL}\big(q(\pmb{z}) || p(\pmb{z} | \pmb{x})\big) = \mathbb{E}\_{q(\pmb{z})} \big[ \log \big( q(\pmb{z}) \big) \big] \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{z},\pmb{x}) \big) \big] + \log \big( p(\pmb{x}) \big) $
     
2. $\text{ELBO}(q) = - \mathbb{E}\_{q(\pmb{z})} \big[ \log \big( q(\pmb{z}) \big) \big] \mathbb{E}\_{q(\pmb{z})} \big[ \log \big(p(\pmb{z},\pmb{x}) \big) \big]$

Combining 1 and 2 gives us:    

$$ D\_{KL}\big(q(\pmb{z}) || p(\pmb{z} | \pmb{x})\big) = \log \big( p(\pmb{x}) \big) - \text{ELBO}(q) \\ \Leftrightarrow \log \big( p(\pmb{x}) \big) = D\_{KL}\big(q(\pmb{z}) || p(\pmb{z} | \pmb{x})\big) + \text{ELBO}(q)$$ 

Because the KL divergence is always non-negative, i.e. $D_{KL}(\cdot) \ge 0$ we know that

$$log \big( p(\pmb{x}) \big) \ge \text{ELBO}(q)$$

Hence, the ELBO provides a lower bound on the (log) evidence $p(\pmb{x})$.

## Example variational family

The choice of the variational family $\mathcal{Q}$, or rather its complexity determines how complex it will be to optimize the ELBO. In simple terms: if the variational family is very complex, it will be more difficult to solve our optimization problem. One way of restricting the variational family $\mathcal{Q}$ is to choose a parametric distribution $q(\pmb{z} \, | \, \theta)$ which is goverend by a set of parameters $\theta$. For example, we could choose a Gaussian distribution. 

Another popular approach is the so called *mean field approximation*. This approach assumes that the variational distribution *factorizes* with respect to some partition of the latent variables. Mean field approximation works as follows:

1. We partition the latent variables $\pmb{z}$ into $M$ disjoint groups $\pmb{z}_i$ with $i = 1, ..., M$. 
2. We then assume that the variational distribution factorizes with respect to these groups:
$$q(\pmb{z}) = \prod_{i=1}^{M} q_i(\pmb{z}_i)$$

We don't make any further assumptions about the form of the different $q_i$. They might all be Gaussian distributions or a combination of different distributions. This offers a lot of flexibility. The goal of variational inference is now to find the distribution $q(\pmb{z})$ of the form $q(\pmb{z}) = \prod_{i=1}^{M} q_i(\pmb{z}_i)$ which maximizes the ELBO. To be more precise, we need to optimize the ELBO with respect to all distributions $q_i(\pmb{z}_i)$. This is done by optimizing with respect to each of the factors in turn. More details on this procedure can be found in Bishop's Pattern Recognition and Machine Learning book in chapter 10.1 (a link to the book is given below).

## Sources and further reading 
- Book: [Pattern Recognition and Machine Learning](https://www.microsoft.com/en-us/research/people/cmbishop/#!prml-book)
- Paper: [Variational Inference:  A Review for Statisticians](https://arxiv.org/pdf/1601.00670.pdf)


---
Date: 2023-12-10
---
Steps to MLE, assuming IID data points $X_i, \dots, X_m$﻿

1. Define the model for the distribution of sample → $X \sim \text{Poi}(\theta)$﻿
2. What is the likelihood of one $X_i$﻿ → PMF / PDF of $X_i$﻿ → $f(X_i = x_i | \theta)$﻿
3. What is the likelihood of all data → $L(\theta) = \Pi_{i=1}^m f(X_i = x_i | \theta)$﻿
4. What is the log-likelihood of all data → $LL(\theta) = \sum_{i=1}^m \log f(x_i | \theta)$﻿
5. Differentiate LL with respect to theta → $\frac{\partial LL(\theta)}{\partial \theta} = \sum_{i=1}^m\frac{\partial}{\partial \theta} \log f(x_i |\theta)$﻿
6. Set derivative to zero or use gradient ascent to find theta that maximizes the log likelihood

  

  

## Rejection Sampling

$P(\mathbf{Q = q} | \mathbf{O = o}) = \frac{P(\mathbf{Q = q} , \mathbf{O = o})}{P(\mathbf{O = o})} = \frac{\text{\# of samples with \textbf{Q = q} and \textbf{O = o}}}{\text{\# of samples with \textbf{O = o}}}$

  

```Python
def prob_query_given_obs(query, obs):
    # generate many samples from the joint distribution
    samples = generate_many_joint_samples(10000)
    
    # keep only samples where Obs = obs, reject samples that
    #   don't align with observation
    keep_samples = []
    for sample in samples:
        if check_obs_match(sample, obs):
            keep_samples.append(sample)
    
    # count samples where Query = query and Obs = obs
    count_consistent = 0
    for sample in keep_samples:
        if check_query_match(sample, query):
            count_consistent += 1

    return count_consistent / len(keep_samples)
```

$$\begin{aligned}L(\theta) &= \prod_{i=1}^n P(Y=y^{(i)}|X = \mathbf{x}^{(i)}) \\ &= \prod_i\sigma(\theta^Tx^{(i)})^{y^{(i)}} *[1-\sigma(\theta^Tx^{(i)})]^{1-y^{(i)}}\\ LL(\theta) &= \sum_i^ny^{(i)}\log\hat{y}^{(i)}+(1-y^{(i)})\log[1-\hat{y}^{(i)}]\end{aligned}$$

## Ethics

**Protected Groups:** Race, color, national origin, religion, age, sex (gender), sexual orientation, physical or mental disability, and reprisal.

**Procedural Fairness:** Focuses on the decision- making or classification process, ensures that the algorithm does not rely on unfair features.

**Distributive Fairness:** Focuses on the decision-making or classification _outcome,_ ensures that the distribution of good and bad outcomes is equitable.

**Parity and Calibration**: Let $G$﻿ be the guess of our model ($\hat{y}$﻿), let $T$﻿ be the true value ($y$﻿), and let $D$﻿ be a protected demographic.

$$\begin{aligned}\textbf{Parity: }& P(G = 1 | D = 1) = P(G = 1 | D = 0) \\\textbf{Calibration: }&P(G = T | D = 1) = P(G = T | D = 0) \\\textbf{Relaxed Calibration: }&\frac{P(G = T | D = 1)}{P(G = T | D = 0)} \geq 1 -\epsilon \text{, where $\epsilon$ = 0.2}\end{aligned}$$
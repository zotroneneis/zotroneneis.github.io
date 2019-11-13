---
title: "Interpretable Outcome Prediction with Sparse Bayesian Neural Networks in Intensive Care"
collection: publications
permalink: /publication/interpretable-outcome-prediction
excerpt: 'Motivated by the potential benefits of a system that accelerates the process of writing radiological reports, we present a Recurrent Neural Network Language Model for modeling radiological language.  We show that Recurrent Neural Network Language Models can be used to produce convincing radiological reports and investigate how their performance can be improved by using advanced regularization techniques like embedding dropout or weight tying, and advanced initialization techniques like pre-trained word embeddings. Furthermore, we study the use of transfer learning to create topic-specific language models. To test the applicability of our techniques to other domains we perform experiments on a second dataset, consisting of forum posts on motorized vehicles. In addition to our experiments on Recurrent Neural Network Language Models, we train a Continuous Bag-of-Words model on the radiological dataset and analyze the resulting medical word embeddings. We show that the embeddings encode medical relationships, semantic similarities and that certain medical relationships can be represented as linear translations.'
date: 2019-05-08
paperurl: 'https://arxiv.org/pdf/1905.02599.pdf'
---


Abstract:
Clinical decision making is challenging because of pathological complexity, as well as large amounts of heterogeneous data generated as part of routine clinical care. In recent years, machine learning tools have been developed to aid this process. Intensive care unit (ICU) admissions represent the most data dense and time-critical patient care episodes. In this context, prediction models may help clinicians determine which patients are most at risk and prioritize care. However, ﬂexible tools such as artiﬁcial neural networks (ANNs) suﬀer from a lack of interpretability limiting their acceptability to clinicians. In this work, we propose a novel interpretable Bayesian neural network architecture which oﬀers both the ﬂexibility of ANNs and interpretability in terms of feature selection. In particular, we employ a sparsity inducing prior distribution in a tied manner to learn which features are important for outcome prediction. We evaluate our approach on the task of mortality prediction using two real-world ICU cohorts. In collaboration with clinicians we found that, in addition to the predicted outcome results, our approach can provide novel insights into the importance of diﬀerent clinical measurements. This suggests that our model can support medical experts in their decision making process.

[Download paper here](https://arxiv.org/pdf/1905.02599.pdf)

<!-- Recommended citation: Your Name, You. (2009). "Paper Title Number 1." <i>Journal 1</i>. 1(1). -->

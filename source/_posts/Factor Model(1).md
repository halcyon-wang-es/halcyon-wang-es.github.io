---
title: Introduction to factor models
date: 2026-03-24
tags:
  - Factor Model
  - PCA
categories:
  - Statistics
  - Econometrics
katex: true
toc: true
description: A note on factor-model representation, PCA estimation, and identification.
---

## Why Factor Models?

​	In high-dimensional data, dependence among observed variables often appears as broad co-movement rather than as a collection of isolated pairwise relationships. In asset pricing, for example, the returns of a large panel of assets are often understood as reflecting exposure to a relatively small number of common risk factors. In macroeconomics, similarly, the joint behavior of many aggregate indicators can often be traced to a limited set of latent forces that capture broad economic conditions. These examples motivate factor models, which assume that much of the dependence among observed variables is driven by a low-dimensional common component. By decomposing variation into common and idiosyncratic parts, factor models provide a parsimonious and interpretable framework for explaining the ==covariance structure== of large datasets, attributing the co-movement of many observed variables to a small number of latent common factors.



## Factor Model Representation

​	To formalize the factor model, let $X \in \mathbb{R}^{T \times N}$ denote the observed data matrix, where $T$ is the number of time periods and $N$ is the number of observed units. In financial applications, the columns of $X$ may correspond to individual assets and the rows to time periods; in macroeconomic applications, the columns may represent economic indicators observed over time. The factor model admits two equivalent representations, depending on whether one views the data column-wise or row-wise.

### **Column-wise representation**

​	First, consider the data from the perspective of an individual unit. Let $x_i \in \mathbb{R}^T$ denote the time series of observations for unit $i$, where $i = 1, \dots, N$. The factor model is written as
$$
x_i = F\lambda_i + \epsilon_i.
$$
Here, $F \in \mathbb{R}^{T \times r}$ is the matrix of common factors, $\lambda_i \in \mathbb{R}^r$ is the loading vector for unit $i$, and $\epsilon_i \in \mathbb{R}^T$ is an idiosyncratic component. The integer $r$ denotes the number of latent factors, typically with $r \ll N$.

​	Under this representation, the entire time path of unit $i$ is decomposed into two parts: a common component, $F\lambda_i$, driven by the latent factors, and a unit-specific residual component, $\epsilon_i$. In this sense, ==the loading vector $\lambda_i$ determines how strongly unit $i$ responds to each common factor over time==.

​	Stacking these series across units yields the matrix form
$$
X = F\Lambda^\top + E,
$$
where $X = [x_1,\dots,x_N] \in \mathbb{R}^{T \times N}, \qquad E = [\epsilon_1,\dots,\epsilon_N] \in \mathbb{R}^{T \times N},$ and $\Lambda = \begin{bmatrix} \lambda_1^\top \\ \vdots \\ \lambda_N^\top \end{bmatrix} \in \mathbb{R}^{N \times r}.$

Thus, the $i$-th column of $X$ is $x_i$, and the $i$-th row of $\Lambda$ is $\lambda_i^\top$.

### **Row-wise representation**

​	An equivalent representation is obtained by indexing the data by time. Let $x_t \in \mathbb{R}^N$ denote the cross-sectional vector of observations at time $t$, where $t = 1, \dots, T$. Then the factor model can be written as
$$
x_t = \Lambda f_t + e_t,
$$
where $f_t \in \mathbb{R}^r$ is the vector of latent common factors at time $t$, $\Lambda \in \mathbb{R}^{N \times r}$ is the loading matrix, and $e_t \in \mathbb{R}^N$ is the idiosyncratic error vector.

​	In this form, the observed cross section at time $t$ is decomposed into a common component, $\Lambda f_t$, and an idiosyncratic component, $e_t$. ==The factor vector $f_t$ captures the realization of the common forces at date $t$, while the loading matrix $\Lambda$ describes how those forces are transmitted to the observed variables.==

​	Stacking these vectors across time again gives
$$
X = F\Lambda^\top + E,
$$
where now $F \in \mathbb{R}^{T \times r}$ is the matrix whose $t$-th row is $f_t^\top$, and $E \in \mathbb{R}^{T \times N}$ is the matrix whose $t$-th row is $e_t^\top$. Equivalently, the $t$-th row of $X$ is $x_t^\top$.

### **Relationship between the two views**

These two perspectives are equivalent and can be summarized by the same matrix representation:
$$
X = F\Lambda^\top + E.
$$
​	The matrix $F\Lambda^\top$ is a low-rank common component, while $E$ collects the idiosyncratic variation not explained by the factors. ==The central idea of the model is therefore that the dependence among many observed series can be summarized by a small number of latent common factors.==



## A Low-Rank View: Factor Models and PCA

​	A natural intuition for estimating the common component in the factor model comes from principal component analysis (PCA). Given the matrix decomposition $X = F\Lambda^\top + E$, the term $F\Lambda^\top$ represents a low-rank component that captures the common variation shared across observed series, while $E$ collects the remaining idiosyncratic variation. If the co-movement in the data is driven by a small number of latent forces, then a low-rank approximation to $X$ should recover much of this common structure. From this perspective, PCA provides a natural empirical starting point for estimating the latent factor space.

​	More formally, PCA estimates the common component by solving a low-rank approximation problem:
$$
(\widehat{F},\widehat{\Lambda}):=\min_{F,\Lambda} \|X - F\Lambda^\top\|_F^2,
$$
where $F \in \mathbb{R}^{T\times r}, \Lambda \in \mathbb{R}^{N\times r}$, and $\|\cdot\|_F$ denotes the Frobenius norm. Since $\widehat{F}\widehat{\Lambda}^\top$ has rank at most $r$, this amounts to finding the best low-rank approximation to the data matrix $X$ in terms of squared reconstruction error. The fitted matrix $\widehat{F}\widehat{\Lambda}^\top$ is then interpreted as the estimated common component, while the residual matrix $\widehat{E} = X - \widehat{F}\widehat{\Lambda}^\top$ captures the variation left unexplained by the low-rank structure.

​	In practice, this estimator is obtained from the leading principal components of the data matrix. The estimated factors span the dominant low-dimensional subspace of $X$, and the associated loadings describe how each observed series is related to that subspace. 

​	However, this interpretation also points to an important identification issue. The decomposition $F\Lambda^\top$ is not unique: for any invertible $r \times r$ matrix $H$,
$$
F\Lambda^\top = (FH)(\Lambda H^{-T})^\top.
$$
As a result, the factors and loadings cannot in general be identified separately without additional normalization or structural restrictions. What PCA recovers is therefore not a unique set of latent factors in a structural sense, but rather the low-dimensional subspace that captures the dominant common variation in the data. This is why PCA is best understood as identifying the factor space, rather than uniquely determining the factors and loadings themselves.

​	This limitation highlights an important gap between PCA and a full factor model: PCA identifies a common low-dimensional subspace, but additional assumptions are needed if one wants a more structured interpretation of factors and loadings.



## Identification and the PCA Estimator

​	To make the role of normalization more explicit, it is useful to start from the singular value decomposition of $X$. Let
$$
X = UDV^\top
$$
be the singular value decomposition, and let $U_r, D_r,$ and $V_r$ denote the matrices formed by the first $r$ singular vectors and singular values. Then the rank-$r$ PCA approximation to $X$ is
$$
\widehat X = U_r D_r V_r^\top.
$$
This representation makes clear that the singular values in $D_r$ may be absorbed either into the estimated factors or into the estimated loadings, depending on the normalization convention.

​	One common choice is to normalize the factors so that
$$
\frac{1}{T}\widehat F^\top \widehat F = I_r.
$$
Under this convention, one may define
$$
\widehat F = \sqrt{T}\,U_r, \qquad \widehat \Lambda = \frac{1}{\sqrt{T}}V_r D_r.
$$
Then
$$
\widehat F \widehat \Lambda^\top = (\sqrt{T}\,U_r)\left(\frac{1}{\sqrt{T}}V_r D_r\right)^\top = U_r D_r V_r^\top = \widehat X,
$$
and
$$
\frac{1}{T}\widehat F^\top \widehat F = \frac{1}{T}(\sqrt{T}U_r)^\top(\sqrt{T}U_r) = U_r^\top U_r = I_r.
$$
In this parameterization, the factors are normalized to have identity sample second moment, while the loadings absorb the singular values.

​	An alternative convention is to normalize the loadings so that
$$
\frac{1}{N}\widehat \Lambda^\top \widehat \Lambda = I_r.
$$
In that case, one may instead define
$$
\widehat \Lambda = \sqrt{N}\,V_r, \qquad \widehat F = \frac{1}{\sqrt{N}}U_r D_r.
$$
Again,
$$
\widehat F \widehat \Lambda^\top = \left(\frac{1}{\sqrt{N}}U_r D_r\right)(\sqrt{N}V_r)^\top = U_r D_r V_r^\top = \widehat X,
$$
while
$$
\frac{1}{N}\widehat \Lambda^\top \widehat \Lambda = \frac{1}{N}(\sqrt{N}V_r)^\top(\sqrt{N}V_r) = V_r^\top V_r = I_r.
$$
Under this convention, the loadings are normalized, and the factors absorb the singular values instead.

​	These two normalizations lead to different numerical representations of $\widehat F$ and $\widehat \Lambda$, but they generate the same fitted common component $\widehat X$. This reflects the broader identification issue in factor models: without additional restrictions, factors and loadings are only determined up to rotation and rescaling. What PCA recovers is therefore best understood as the estimated low-dimensional factor space and the common component it generates, rather than a unique structural factorization.

​	This limitation highlights an important gap between PCA and a full factor model. PCA provides a canonical estimator of the common low-rank component, but additional assumptions are needed if one wants to attach a more structured statistical interpretation to the factors and loadings. 
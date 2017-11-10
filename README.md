Scalable Log Determinants for Gaussian Process Kernel Learning
===============

This repository contains the experiments of the [Scalable Log Determinants for Gaussian Process Kernel Learning](PLACEHOLDER) by Kun Dong, David Eriksson, Hannes Nickisch, David Bindel, Andrew Gordon Wilson. This paper will be appearing at NIPS 2017.

The bibliographic information for the paper is
```bibtex
@inproceedings{dong2017logdet,
  title={Scalable Log Determinants for Gaussian Process Kernel Learning},
  author={Dong, Kun and Eriksson, David and Nickisch, Hannes and Bindel, David and Wilson, Andrew Gordon},
  booktitle={Advances in Neural Information Processing Systems},
  year={2017}
}
```



## Contents
1. [Introduction](#introduction)
2. [Setup](#setup)
4. [Usage](#usage)
    1. [Hickory](#hickory)
    2. [Sound](#sound)
    3. [Crime](#crime)
    4. [Precipitation](#precipitation)

## Introduction
 
The computation of the log determinant with its derivatives for positive definite kernel matrices appears in many machine learning applications, such as Bayesian neural networks, determinantal point processes, elliptical graphical models, and kernel learning for Gaussian processes (GPs). Its cubic-scaling cost becomes the computational bottleneck for scalable numerical methods in these contexts. Here we propose a few novel linear-scaling approaches to estimate these quantities from only fast matrix vector multiplications (MVMs). Our methods are based on stochastic approximations using Chebyshev, Lanczos, and surrogate models. We illustrate the efficiency, accuracy and flexibility of our approaches with experiments covering

1. Various kernel types, such as square-exponential, Matérn, and spectral mixture.
2. Both Gaussian and non-Gaussian likelihood.
3. Previously prohibitive data size.
<!---4. High-dimensional feature space.--->


## Setup

1. Clone this repository
2. Run startup.m

## Usage

The code is built with Matlab. In particular, the Lanczos algorithm with reorthogonalization uses ARPACK which comes with Matlab by default. If you are using Octave and require reorthogonalization, replace *lanczos_arpack* with *lanczos_full* in *cov/apx.m*. Fast Lanczos implementation without reorthogonalization is also available by setting *ldB2_maxit = -niter*. Please check the comments in demos for example.

### Hickory Tree Distribution

In this experiment we apply a log-Gaussian Cox model with the RBF kernel and the Poisson likelihood to a point pattern of 703 Hickory tree. The dataset comes from the R package spatstat.

To run this experiment you can use the `demo_hickory` command. This should take about a minute, and produce the figure below.

<p align="center">
    <img src="https://user-images.githubusercontent.com/21109870/32645169-d8f333e8-c5b3-11e7-8159-4c0544bbcc4e.png" width="700">
</p>

You will see that the Lanczos method produces qualitatively superior prediction than the scaled eigenvalues + Fiedler method. Flexibility is a key advantage of the Lanczos method since it only uses fast MVMs, while scaled eigenvalues method does not directly apply to non-Gaussian likelihoods, hence requires additional approximations.

### Natural Sound Modeling

In this experiment we try to recover contiguous missing regions in a waveform with n = 59306 data points. Use the command `demo_sound` with inputs
1. method: 'lancz' (default), 'cheby', 'ski', 'fitc'
2. ninterp: Number of interpolative grid points. Default: 3000.

Here the accuracy of the prediction is mostly controlled by the number of grid points you use for structured kernel interpolation (SKI). A reasonable range will be between 3000 and 5000. If you run the full experiment setting stored in the data file, you can produce the figure below. As the grid size grows, even the quadratic-scaling cost for scaled eigenvalues method can become prohibitive, yet our linear-scaling method remains efficient.

<p align="center">
    <img src="https://github.com/kd383/GPML_SLD/blob/master/demo/sound/sound_data.pdf" width="200">
    <img src="https://github.com/kd383/GPML_SLD/blob/master/demo/sound/sound_recovery.pdf" width="200">
    <img src="https://github.com/kd383/GPML_SLD/blob/master/demo/sound/sound_inference.pdf" width="200">
    <img src="https://github.com/kd383/GPML_SLD/blob/master/demo/sound/sound_smae.pdf" width="200">
</p>


### Crime

### Precipitation
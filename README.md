# ACD-EDMD

This repository is the primary brach for the implementation of ACD-EDMD. 
The paper has been accepted to be published in RA-L and is available at  <https://arxiv.org/abs/2111.12256>.


## Overview

We provide two main examples of implementations: the differential drive robot and the robot arm.
Other than ACD-EDMD, we also give the code of other approaches.

1) Classical EDMD with dictionary that contains the direct sum of Hermite polynomials of up to second-order terms in all states; 
2) EDMD-DL with 25 dictionary outputs (includes one constant non-trainable map, the coordinate projection non-trainable maps and the rest trainable lifting functions) [1]; 
3) SINDy with nonlinear functions introduced in [2] solved with least absolute shrinkage and selection operator (LASSO).

In the main file, it is running ACD-EDMD now and if you want to try other approaches, minor changes should be made accordingly. 
As the performance evaluation differs among cases, we don't give the script while providing the functions to get prediction from the estimated Koopman operator (which is the current output of the main file.)

**NOTE:** As ACD-EDMD extends the states in some cases, please don't forget to use the original states if you want to implement SINDy or EDMD with Hermite polynomials.
As for the EDMD-DL, we provide the complete code that you could run directly.

## Instruction

We will introduce the usage of each script.

### Initialization

``initialization.m``: It is used to collect data and calculate the estimated Koopman operator. 
It will be launched directly if you run the main file. To compare the performance, we give the ``INPUT.mat`` for all the approaches. 
You can try your own inputs and noise could also be included (which is set to be zero in the current version).

``init_EDMDDL.m``: As the initialization of deep learning is too different, we write the individual code for the initialization of EDMD-DL.

``diffDriveCT.m``, ``diffDriveDT.m``, ``rarmCT.m`` and ``rarmCT.m`` are the dynamics of differential drive robot and the robot arm. They are **only** used when we collect the training data set.

### Estimation of operator

- ACD-EDMD and EDMD with Hermite polynomials

  ``predictionEDMD.m``: It is the function to estimate the Koopman operator from data using EDMD [3]

  ``dictionary.m``: That script lists all the candidate dictionaries of lifting functions. 
  
- EDMD-DL
  
  ``FFnet.m``: The dictionary of lifting functions learned by the deep neural network.
  
  ``EDMDDL_K.m``: The function of forward propagation that estimates the operator using current NN.
  
  ``EDMDDL_phi.m``: The function of back propagation that uses current K to update the weighting matrix and bias. 

- SINDy

  ``SINDy.m``: It is the function to estimate the matrix Xi. (Note that SINDy [2] is not estimating the Koopman operator but the other approach to extract governing equations of nonlinear systems. We introduce them together just for convenience.) 
 
  ``Dic_sindy.m``: The candidate nonlinear functions used in SINDy.
  
### Prediction using the learned dynamics

``wMatrix.m``: It lists the weighting matrix B corresponding to each dictionary. [3]

``doubleV.m``: It is the calculation of matrix F which is required in a simplification version of prediction calculation [4].

``koopEst.m``: It's the script to predict the next states using EDMD [3] [4]. 

``sindyEst.m``: It's the script to predict the next states using SINDy [2].

``DLEst.m``: It's the script to predict the next states using EDMD-DL [1].

## Reference: 

[1] Q.  Li,  F.  Dietrich,  E.  M.  Bollt,  and  I.  G.  Kevrekidis,  “Extendeddynamic mode decomposition with dictionary learning: A data-drivenadaptive  spectral  decomposition  of  the  koopman  operator,”Chaos:An  Interdisciplinary  Journal  of  Nonlinear  Science,  vol.  27,  no.  10,p. 103111, 2017.

[2] S. L. Brunton, J. L. Proctor, and J. N. Kutz, “Discovering governingequations  from  data  by  sparse  identification  of  nonlinear  dynamicalsystems,”Proceedings of the national academy of sciences, vol. 113,no. 15, pp. 3932–3937, 2016.

[3] M. O. Williams, I. G. Kevrekidis, and C. W. Rowley, “A data–drivenapproximation  of  the  koopman  operator:  Extending  dynamic  modedecomposition,”Journal  of  Nonlinear  Science,  vol.  25,  no.  6,  pp.1307–1346, 2015.

[4] L. Shi and K. Karydis, Enhancement for Robustness of Koopman Operator-based Data-driven Mobile Robotic Systems.  In IEEE Int. Conf. on Robotics and Automation (ICRA), 2021.

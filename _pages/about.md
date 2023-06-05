---
permalink: /
title: Perceptual Uncertainty for Marine Autonomy (PUMA)
excerpt: "About me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

In the University of Michigan Field Robotics Group ([FRoG](https://fieldrobotics.engin.umich.edu/)), one of our research directions is the perceptual uncertainty fo marine autonomy. Selected projects are listed below.

<!-- buttons: robot, field work, paper, about -->
<!-- we can talk about methods and results in about.md, and set up other .md files for  robot, field work, paper-->
## ⭐ Uncertainty-Aware Acoustic Localization and Mapping for Underwater Robots

<b>The robot operating in a wave basin with ground truth 3D scan shown in black. </b>
<p float="middle">
  <img src="./images/intro.png" width="100%" />
</p>

We show a pitch introduction figure as an overview of the proposed method below.
<p float="middle">
  <img src="./images/Proposed_method_pitch.svg" width="100%" />
</p>

For underwater vehicles, robotic applications have the added difficulty of operating in highly unstructured and dynamic environments.
Environmental effects impact not only the dynamics and controls of the robot but also the perception and sensing modalities.
Acoustic sensors, which inherently use mechanically vibrated signals for measuring range or velocity, are particularly prone to the effects that such dynamic environments induce.

This paper presents an uncertainty-aware localization and mapping framework that accounts for induced disturbances in acoustic sensing modalities for underwater robots operating near the surface in dynamic wave conditions.
For the state estimation task, the uncertainty is accounted for as the added noise caused by the environmental disturbance.
The mapping method uses an adaptive kernel-based method to propagate measurement and pose uncertainty into an occupancy map.
Experiments are carried out in a wave tank environment to perform qualitative and quantitative evaluations of the proposed method.

<!-- TODO: add one or two videos might be cool -->

### Method Overview

✅: We characterize the uncertainty induced on acoustic sensors by external disturbances i.e. waves.

✅: We integrate the uncertainty induced from external disturbances into a localization and mapping framework for marine robot platforms.

✅: We provide an extension of previous work on continuous 3D mapping to the underwater domain while contributing a novel, adaptive sparse kernel design for 3D mapping to enable uncertainty propagation from uncertain pose estimates into a 3D occupancy map.

### Experiment

Experiments were run in the [Marine Hydrodynamics Lab (MHL)](https://mhl.engin.umich.edu/) at the University of Michigan to perform this characterization.

We conducted experiments for:

- Quantifying the effect of different wave conditions on the sensor noise and biases.

It is essential to characterize the sensor noise associated with the robot operating under different environmental conditions.
Experiments were run in the Marine Hydrodynamics Lab (MHL) at the University of Michigan to perform this characterization.
We specifically conducted experiments to quantify the effect of different wave conditions on the sensor noise and biases.
To properly evaluate the proposed method, we rigidly mounted the robot to a carriage and moved the carriage in wave conditions.
The carriage positions are recorded and used to serve as ground truth reference for robot trajectory.
Time synchronization across different systems was conducted to ensure proper evaluation with the ground truth data.

To measure the external disturbances of waves on the robot, we recorded the wave characteristics from a wave probe. The measurements are used to characterize the induced additive noise from external disturbances.

The obtained empirical mean and variance of the measurements are incorporated into the measurement models of the filtering formulation.
<!-- <figure class="video_container" float="center">
<iframe width="256" height="459" src="https://www.youtube.com/embed/wJtMK4djHKc" title="UM Frog Wave Test - Waves" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</figure> -->

### Results

For our results, we present them for the two technical sections:

#### State Estimation Evaluation

In order to evaluate the state estimation performance, we use the reference trajectory the robot followed during the wva tests that we ran.
We obtained this reference trajectory from the positions logged from the linear carriage at the MHL.

We use a standard UKF as a baseline method, to highlight the improvements of our uncertainty-aware and bias accounting approach over a "vanilla" method.

|      Method     | x (m) | y (m) | z (m) | $\phi$ (rad) | $\theta$ (rad) | $\psi$ (rad) | $v_x$ (m/s) | $v_y$ (m/s) | $v_z$ (m/s) |
|:---------------:|:-----:|:-----:|:-----:|------------|-------------|-----------|-------------|-------------|-------------|
| Baseline UKF    | 0.862 | 0.297 | 0.004 | 0.003      | 0.003       | 0.015     | 0.023       | 0.007       | 0.003       |
| Proposed Method | 0.869 | 0.012 | 0.004 | 0.003      | 0.003       | 0.003     | 0.020       | 0.005       | 0.003       |

We show that our method drastically improves the pose estimate of the robot, significanly reducing teh drift in the estimation.

We additioonally point the improvement on the directly measured variables, primarily $v_x$, $v_y$ and $v_z$. We obtain the ground truth velocity of the robot, again, by using the position of the linear carriage, and differentiate it over time. The improvement on the directly observed variables is a key addition by the uncertainty-aware approach. When there was an impact by the environmental disturbances, the sensors were trusted less. This led to a filtered velocity estimation, that additionally results in a more accurate estimation.

#### Seafloor Mapping Evaluation

To evaluate the seafloor mapping performance, we again use a reference for our method. For this, we use a previously generated, dense 3D model of the bottom of the wave basin.

As we construct a 3D occupancy grid map, we treat the voxelized map as a point cloud, and compare it with the reference pointcloud to evaluate the pointcloud-to-pointcloud distance. We additionally measure the pointcloud density.

As a baseline, we compare the mapping method to a standard 3D counting sensor model, to highlight the improvement of our kernel based approach.

We show the results on the density and the accuracy are greatly improved using our method, leading to a more informative map.

| Method | Pointcloud to Pointcloud Distance (m) | Map Density |
|:---:|:---:|:---:|
| Baseline CSM | 0.226 | 2774 |
| Proposed Method | 0.087 | 3190 |

We also show the capability of our mapping method to capture details of the seabaed by mapping a rock platform with known dimensions, shown in the figure below.

<p float="middle">
  <img src="./images/rock_pc.png" width="100%" />
</p>
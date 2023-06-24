---
title: "[Paper]3D Vision Papers"
toc: true
toc_sticky: true
toc_lable: "Main Contents"
use_math: true
categories:
  - Computer Vision
tags:
  - [3D vision]
---

# Shape representation

## Depth Maps

> (NIPS 14') Depth Map Prediction from a Single Image using a Multi-Scale Deep Network

- Loss : Scale Invariant error

$D(y,y^*)={1\over n} \Sigma_i d_i^2 - {1\over n^2} \Sigma_{i,j} d_id_j$

- Archtiecture:  for Multi scale Image
[Image]

> (ICCV 15') Predicting Depth, Surface Normals and Semantic Labels with a Common Multi-Scale Convolutional Architecture

- Problem Definition
Input : 2D Image<br>
Output : Depth Predction, Surface Normal Estimation, Labels(for every pixel)

- Idea
3 Task at once: Depth Prediction, Surface Normal Estimation, Semantic Segmentation <br>
Generate **directly** from input image


- Limitation

Every task requires a supervised learning. Therefore ground truth value is needed!


- Loss 

Of course, 3 different tasks have task specific loss!

Depth : Scale Invariant Loss Again!<br>
Surface Normal : <br>
$L_{normals}(N,N^*) = - {1\over n} \sum_i N_i\cdot N_i^* = -{1\over n} N\cdot N^*$<br>
$N$ : predicted normal vector map<br>
$N^*$: ground true normal vector map

Calculate normal vector component at (x,y,z): from 1 channel into 3 channel!

Semantic Lables<br>
$L_{semantic}(C,C^*) = -{1\over n} \sum_i C_i^*\log (C_i)$

Pixelwise softmax classifiers!

- train

Supervised learning! Every task use supervised learning


- Architecture : Multi-scale 

[Archtecture]

>  How to deliver gradiient(updates) for every scale-specific network?

Key is that how to train whole network. For now, it is natural to use an input as the previous layer output. 



## Voxel Grid

> (CVPR 15') 3D ShapeNets: A Deep Representation  for Volumetric Shapes

- Problem Definition

Input : 2D Image + Depth value<br>
Output: Shape Completion

- Idea
Train generic shape representation<br>
Geometric 3D shape as a ** probabilistic distribution of binary variable** on a 3D voxel grid

- Limitation

Binary representation of 3D is large and sparse. Scalability might be problematic.

- Loss

- Train

- Downstream task

Because it is a probabilistic model, there are somewhat different inference process!

- Architecture: Convolution for reducing parameters

## Point Cloud

- Problem Definition

- Idea

- Limitation

- Loss

- train

- Architecture

## Mesh

## Implicit surface

# View Synthesis

## NeRF

## Dynamic NeRF

## Generalizable NeRF

## RawNeRF




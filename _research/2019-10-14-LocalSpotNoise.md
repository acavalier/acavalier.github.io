---
layout: single
classes: wide
title: "Local Spot Noise for Procedural Surface Details Synthesis"
excerpt: "We propose a procedural texturing method based on the locally controlled spot noise providing *on-the-fly anisotropic filtering* and *on-the-fly normal mapping* to add visuals details on surfaces. Accepted at **Computer & Graphics** (2019)."
permalink: /research/local-spot-noise-paper
header:
  teaser: assets/images/research/lsn/result_dragon.jpg
gallery1:
  - url: /assets/images/research/lsn/pattern_1.png
    image_path: assets/images/research/lsn/pattern_1.jpg
    alt: "Spot Noise Pattern 1"
  - url: /assets/images/research/lsn/pattern_2.png
    image_path: assets/images/research/lsn/pattern_2.jpg
    alt: "Spot Noise Pattern 2"
  - url: /assets/images/research/lsn/pattern_3.png
    image_path: assets/images/research/lsn/pattern_3.jpg
    alt: "Spot Noise Pattern 3"
gallery2:
  - url: /assets/images/research/lsn/result_dragon.png
    image_path: assets/images/research/lsn/result_dragon.jpg
    alt: "Dragon - Composite Texture"
  - url: /assets/images/research/lsn/result_buddha.png
    image_path: assets/images/research/lsn/result_buddha.jpg
    alt: "Buddha - Composite Texture"
  - url: /assets/images/research/lsn/result_gargoyle.png
    image_path: assets/images/research/lsn/result_gargoyle.jpg
    alt: "Gargoyle - Composite Texture"
gallery3:
  - url: /assets/images/research/lsn/stylized_1.png
    image_path: assets/images/research/lsn/stylized_1.jpg
    alt: "Stylized Rendering 1"
  - url: /assets/images/research/lsn/stylized_2.png
    image_path: assets/images/research/lsn/stylized_2.jpg
    alt: "Stylized Rendering 2"
  - url: /assets/images/research/lsn/stylized_3.png
    image_path: assets/images/research/lsn/stylized_3.jpg
    alt: "Stylized Rendering 3"
sidebar:
  - image: assets/images/research/lsn/result_dragon.jpg
    image_alt: "teaser image"
  - title: "Arthur Cavalier"
  - title: "Guillaume Gilet"
  - title: "Djamchid Ghazanfarpour"
    text: "Université de Limoges, CNRS, XLIM, UMR 7252, F-87000 Limoges, France"
---

## Abstract

To deal with the increasing demand for complex visual details in virtual worlds, procedural methods for content authoring are an expanding field in Computer Graphics. Focusing on on-the-fly texture generation, we present in this paper a content authoring process based on Locally Controlled Spot Noise. Through the control of both the impulses distribution and the spatially-defined kernel, this process can cover a wide range of appearances. In this context, we introduce a new kernel formulation that provides an efficient anisotropic filtering of the generated texture. Furthermore, our method allows users to interactively create the desired appearance by controlling both albedo and meso-geometry of the underlying surface, tackling on-the-fly normal map generation. Our method can be used as an artist friendly tool to model high-quality surface details with direct control over the final appearance in real-time.

## Paper & Supplementary materials
* [Computer & Graphics](https://www.sciencedirect.com/science/article/abs/pii/S0097849319301608)
* [PrePrint](https://drive.google.com/open?id=1kDm5cZJr2pfsRdctTNO8oxUDz7LU_dqG) - [Supplementary material](https://drive.google.com/open?id=1O8lPafIZomBJwGNFB_4-B7daHsXkKbYV) - [Video](https://drive.google.com/open?id=1GuYmwLssD9Lh6eX8DyNKeC-lv-Df58QA)
* Shadertoy implementations (tested on firefox): 
    * [On-the-fly Filtering](https://www.shadertoy.com/view/tdyXzK)
    * [On-the-fly Normal mapping](https://www.shadertoy.com/view/Wdc3W7)
    * [Spatially varying pattern using control map](https://www.shadertoy.com/view/Ws33W7)


## Results
{% include gallery id="gallery1" caption="By defining the kernel (using a sum of Gaussians) and controlling the distribution, the user can generated a wide range of patterns." %}
{% include gallery id="gallery2" caption="Results using our spot noise model to add relief and to drive two procedural textures." %}
{% include gallery id="gallery3" caption="Stylized Results using our spot noise." %}

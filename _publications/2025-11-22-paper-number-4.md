---
title: "Diverse Instance Generation via Diffusion Models for Enhanced Few-Shot Object Detection in Remote Sensing Images"
collection: publications
permalink: /publication/2025-11-22-paper-number-4
excerpt: 'This paper proposes a novel framework that leverages diffusion models to synthesize diverse remote sensing instances for improving few-shot object detection performance.'
date: 2025-11-22
venue: 'IEEE Geoscience and Remote Sensing Letters'
paperurl: 'https://arxiv.org/abs/2511.18031'
citation: 'Liu, Yanxing, Jiancheng Pan, Jianwei Yang, Tiancheng Chen, Peiling Zhou, and Bingchen Zhang. "Diverse Instance Generation via Diffusion Models for Enhanced Few-Shot Object Detection in Remote Sensing Images." IEEE Geoscience and Remote Sensing Letters, vol. 22, 2025, pp. 1-5, Art no. 6015405, doi: 10.1109/LGRS.2025.3624148.'
---
Few-shot object detection (FSOD) aims to detect novel instances with only a limited number of labeled training samples, presenting a challenge that is particularly prominent in numerous remote sensing applications such as endangered species monitoring and disaster assessment. Existing FSOD methods for remote sensing images (RSIs) have achieved promising progress but remain constrained by the limited diversity of instances. To address this issue, we propose a novel framework that can leverage a diffusion model pretrained on large-scale natural images to synthesize diverse remote sensing instances, thereby improving the performance of few-shot object detectors. Instead of directly synthesizing complete remote sensing images, we first generate instance-level slices via a specialized slice-to-slice module, and then embed these slices into full-scale imagery for enhanced data augmentation. To further adapt diffusion models for remote sensing scenarios, we develop a class-agnostic image inversion module that can invert remote sensing instance slices into semantic space. Additionally, we introduce contrastive loss to semantically align the synthesized images with their corresponding classes. Experimental results show that our method has achieved an average performance improvement of 4.4% across multiple datasets and various approaches.

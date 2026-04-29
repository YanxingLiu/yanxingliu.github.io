---
title: "EarthSynth: Generating Informative Earth Observation with Diffusion Models"
collection: publications
permalink: /publication/2025-05-17-paper-number-2
excerpt: 'This paper proposes EarthSynth, a diffusion-based generative foundation model that synthesizes multi-category, cross-satellite labeled Earth observation data for downstream remote sensing image interpretation tasks.'
date: 2025-05-17
venue: 'arXiv preprint arXiv:2505.12108'
paperurl: 'https://arxiv.org/abs/2505.12108'
citation: 'Pan, Jiancheng, Shiye Lei, Yuqian Fu, Jiahao Li, Yanxing Liu, Yuze Sun, Xiao He, Long Peng, Xiaomeng Huang, and Bo Zhao. "EarthSynth: Generating Informative Earth Observation with Diffusion Models." arXiv preprint arXiv:2505.12108 (2025).'
---
Remote sensing image (RSI) interpretation typically faces challenges due to the scarcity of labeled data, which limits the performance of RSI interpretation tasks. To tackle this challenge, we propose EarthSynth, a diffusion-based generative foundation model that enables synthesizing multi-category, cross-satellite labeled Earth observation for downstream RSI interpretation tasks. To the best of our knowledge, EarthSynth is the first to explore multi-task generation for remote sensing, tackling the challenge of limited generalization in task-oriented synthesis for RSI interpretation. EarthSynth, trained on the EarthSynth-180K dataset, employs the Counterfactual Composition training strategy with a three-dimensional batch-sample selection mechanism to improve training data diversity and enhance category control. Furthermore, a rule-based method of R-Filter is proposed to filter more informative synthetic data for downstream tasks. We evaluate our EarthSynth on scene classification, object detection, and semantic segmentation in open-world scenarios. There are significant improvements in open-vocabulary understanding tasks, offering a practical solution for advancing RSI interpretation. Project page: [https://jaychempan.github.io/EarthSynth-website](https://jaychempan.github.io/EarthSynth-website).

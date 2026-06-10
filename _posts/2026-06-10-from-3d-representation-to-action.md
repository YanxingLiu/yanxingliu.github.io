---
title: '从表示到行动'
date: 2026-06-10
permalink: /posts/2026/06/from-representation-to-action/
author_profile: false
tags:
  - computer vision
  - world model
  - embodied AI
  - VQA
---

昨天在刷到了[bitter_lesson_of_cv](https://www.vincentsitzmann.com/blog/bitter_lesson_of_cv/) ,Vincent指出,可能3D表示并不是一个real task,最终我们的目标还是action。就和ross也指出 Detection也仅是一个parser一样。 我们会先通过detection来组织出目标框,然后通过一系列的目标框来回答VQA。我们最终的任务从来都不是表示，而是最终的Action。

以这个例子来说,Vincent同样也指出,可能我们也并不需要三维重建,因为即使我们重建出来了三维的表示,我们仍然需要通过这种三维的表示来决定下一步采取的行动。归根结底,我们需要的只是一个未来的action而已,我们并不关心之前的目标究竟是如何formalize的,A目标和B目标之间有多少距离,我们只关心接下来AI会如何进行操作,仅此而已。

在虚拟世界,AI将所有的操作都视为Token,把所有的未来的action都视为Next Token Prediction。通过这样的形式,他可以把任何计算机上的任务接管。这造就了现在LLM的繁荣。

但是在物理世界,我们的未来会是什么样的呢? 

World Model希望能够可控地生成未来的世界状态,也有一些工作将action视为世界的变化,并通过直接的world的变换来产生Action,这可能是一种组织的形式把,但是感觉World到Action的映射相对来说没有LLM中那样的直观,因此目前的效果也并没有那么好。

LLM的pretrain-sft的范式的成功,个人觉得很大程度来源于他可以将action也组织成一种token,这样的话预训练(学习世界知识),和SFT(学习如何采取action)可以很方便的组织在一起。

但是在World Model中,世界知识的组织是图像,因此我们需要通过从不断的一帧帧的图像来学习到这个世界是如何变化的,并且目前已经有不错的进展了。但是如何将图像和机器的action进行统一呢? 通过什么样的表示能够方便地利用World model中世界知识，并将其迁移到Action Prediction的任务中呢？
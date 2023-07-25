---
permalink: /papers
layout: page
title: Recent Work
---

**Principles for Initialization and Architecture Selection in Graph Neural Networks with ReLU Activations**. Preprint: [arxiv:2306.11668](https://arxiv.org/abs/2306.11668).

*This article derives and validates three principles for initialization and architecture selection in finite width graph neural networks (GNNs) with ReLU activations. First, we theoretically derive what is essentially the unique generalization to ReLU GNNs of the well-known He-initialization. Our initialization scheme guarantees that the average scale of network outputs and gradients remains order one at initialization. Second, we prove in finite width vanilla ReLU GNNs that oversmoothing is unavoidable at large depth when using fixed aggregation operator, regardless of initialization. We then prove that using residual aggregation operators, obtained by interpolating a fixed aggregation operator with the identity, provably alleviates oversmoothing at initialization. Finally, we show that the common practice of using residual connections with a fixup-type initialization provably avoids correlation collapse in final layer features at initialization. Through ablation studies we find that using the correct initialization, residual aggregation operators, and residual connections in the forward pass significantly and reliably speeds up early training dynamics in deep ReLU GNNs on a variety of tasks.*

**Graph Neural Networks at the Large Hadron Collider**. DOI: [https://doi.org/10.1038/s42254-023-00569-0](https://doi.org/10.1038/s42254-023-00569-0). 

*From raw detector activations to reconstructed particles, data at the Large Hadron Collider (LHC) are sparse, irregular, heterogeneous and highly relational in nature. Graph neural networks (GNNs), a class of algorithms belonging to the rapidly growing field of geometric deep learning (GDL), are well suited to tackling such data because GNNs are equipped with relational inductive biases that explicitly make use of localized information encoded in graphs. Furthermore, graphs offer a flexible and efficient alternative to rectilinear structures when representing sparse or irregular data, and can naturally encode heterogeneous information. For these reasons, GNNs have been applied to a number of LHC physics tasks including reconstructing particles from detector readouts and discriminating physics signals against background processes. We introduce and categorize these applications in a manner accessible to both physicists and non-physicists. Our explicit goal is to bridge the gap between the particle physics and GDL communities. After an accessible description of LHC physics, including theory, measurement, simulation and analysis, we overview applications of GNNs at the LHC. We conclude by highlighting technical challenges and future directions that may inspire further collaboration between the physics and GDL communities.*



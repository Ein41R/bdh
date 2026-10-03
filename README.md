### JDH, a joint embedding approach to BDH.
*read pathways jepa[https://github.com/pathwaycom/bdh/edit/main/README.md] for details on the architecture*


***

## Motivation

***


>"you cannot have an intelligent agent that doesn't have a world model."
>*Yann LeCun*
As Yann LeCun et al. demonstated in his paper[https://arxiv.org/abs/2301.08243]. The Joint-Embedding Predictive Architecture allows a agent to learn semantic information without a human superviser.

In Comparison to that the BDH model from pathway enables a agent to remember a working memory, from which it can retrieve information that was previously stored within the model.

Both both architectures seem promising at first, however there is one problem. BDH takes the shape a standard Transformer while JEPA is based of two separate encoders that are trained in tandem. In order to bridge this gap, I devoted my free time into working at a working framework of a Model that works with semantic information like JEPA while maintaining the features of pathways BDH.

**NOTE:** I must admin that I am not familiar with best practices in maintaining a public github repository, therefore I'd appreciate any constructive critizism and like to apologize for any inconveniences. Additionaly, this is my first time working on machiene learning, I've set high stakes for myself and I thus I will append a writing documenting my progress on this project.

**Disclaimer** for now I won't add a writeup explaining the underlying architecture, since it is simply a fusion of BDH and JEPA, if you'd like a writeup, feel free to contact me and I will consider writing one in my spare time.

***

## Documentation

***


Now, I am starting this documentation a bit late in the progress. Initially I did not plan to write this documentation, however I have to admit that I seriously underestimated the complications I met during the development.
At the time of writing I am struggling with representation collapse, while working on replacing the contrastive loss approach with a VICReg-based approach leveraging especially variance and covariance.

After reading (AI in Robotics)[https://www.amazon.co.uk/AI-Robotics-Embodied-Intelligence-Physical/dp/B0DFLTZ4SQ] by Alishba Imran, I first spend a significant amount of time covering the working principle of pathways BDH. Before it I was told that BDH works by *"wiring together neurons that fire together"*. Which confused me at first, since my understanding for Artificial intelligence at that time was that it was simple linear algebra and I couldn't make sense of matrices being analogous to individual neurons that are connected. Even though I've read Alishba Imrans book, I still hadn't internalized the functioning mechanism of neurons being the entries within a vector in the feature space that "fire" or get activated through a activation funtion.

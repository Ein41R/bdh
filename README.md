### JDH, a joint embedding approach to BDH.
*read pathways jepa[https://github.com/pathwaycom/bdh/edit/main/README.md] for details on the architecture*


***

## Motivation

***


>"you cannot have an intelligent agent that doesn't have a world model."
>*Yann LeCun*
As Yann LeCun et al. demonstated in his [paper](https://arxiv.org/abs/2301.08249). The Joint-Embedding Predictive Architecture allows a agent to learn semantic information without a human superviser.

In Comparison to that the BDH model from pathway enables a agent to remember a working memory, from which it can retrieve information that was previously stored within the model.

Both both architectures seem promising at first, however there is one problem. BDH takes the shape a standard Transformer while JEPA is based of two separate encoders that are trained in tandem. In order to bridge this gap, I devoted my free time into working at a working framework of a Model that works with semantic information like JEPA while maintaining the features of pathways BDH.

**NOTE:** I must admin that I am not familiar with best practices in maintaining a public github repository, therefore I'd appreciate any constructive critizism and like to apologize for any inconveniences. Additionaly, this is my first time working on machiene learning, I've set high stakes for myself and I thus I will append a writing documenting my progress on this project.

**Disclaimer** for now I won't add a writeup explaining the underlying architecture, since it is simply a fusion of BDH and JEPA, if you'd like a writeup, feel free to contact me and I will consider writing one in my spare time.

***

## Documentation

***


Now, I am starting this documentation a bit late in the progress. Initially I did not plan to write this documentation, however I have to admit that I seriously underestimated the complications I met during the development.
At the time of writing I am struggling with representation collapse, while working on replacing the contrastive loss approach with a VICReg-based approach leveraging especially variance and covariance.

After reading [AI in Robotics](https://www.amazon.co.uk/AI-Robotics-Embodied-Intelligence-Physical/dp/B0DFLTZ4SQ) by Alishba Imran, I first spend a significant amount of time covering the working principle of pathways BDH. Before it I was told that BDH works by *"wiring together neurons that fire together"*. Which confused me at first, since my understanding for Artificial intelligence at that time was that it was simple linear algebra and I couldn't make sense of matrices being analogous to individual neurons that are connected. Even though I've read Alishba Imrans book, I still hadn't internalized the functioning mechanism of neurons being the entries within a vector in the feature space that "fire" or get activated through a activation funtion. Additionally I've noticed that the schema provided by pathway on their github repository is misleading and not neccessarily true, since x is the only variable that really gets passed to the next layer.

It took me quite the time, but eventually I formed a reasonable understanding to work with pytorch. As a result, implementing a EMA copy encoder the initial encoder was fairly easy. I might have been a bit sloppy with my implementation, but that should be good for now. The subsequent implementation of the decoder model was a piece of cake. 

However, making the resulting model learn was a task on its own.

It all began with the output being a peculiar mix of i,j and - symbols. I was not aware of representation collapse at a the time, since I devoted little to no time researching JEPA. This is becuase I managed the previous adjustments to BDH at ease. Since the results were highly unsatisfying I did the most obvious and implemented simple self attention to the predictor model. At the time I hadn't implemented any attention into the predictor. It just didn't seem neccessary and I reasoned myself into using a simple trained positional embedding. This turned out to cost me a lot of time afterwards.

Despite the attention, there was no visible change in the models output. Further experiments revealed that my model had a hard minimal token limit of 64. Which strongly confused me, since the only constants in my model were parameter sizes which couldn't possibly impose a limit to the sequence length. In my confusion I added a zero padding to any input in order for it to meet the minimum context limit. I tried changing the predictor architecture, changing loss, I don't even remember the countless useless changes I've made. I ended up using L1 distance as loss since I hoped it makes the predictor and the ema target as similar as possible.

This time I ended up with dozens of "\x00" as model output. Since the only zero I was aware of was the padding, I tried to find the perfect padding. I though maybe having a padding of 0 makes the subsequent prediction 0 too. But that didn't change anything in the model output. Desperately searching for a answer to my questions, I somehow managed to fix the hard limit, which turned out to be the result of the positional embedding, which was trained for a constant sequence size the length of the target sequence. But that didn't fix the zero problem, so I consulted a llm. GLM-5.3 to be precise.

It was only then that I learned that the core problem was the representation collapse. The intuition is obviously to use negative pairs, so my Idea was that I should train sequences from the same batch to be similar and punish if embeddings from different batches were similar. But I lacked the knowledge to implement it, my designs would never work. Consulting Deepseek v4 flash, it proposed using torch.einsum to calculate dot products as a similarity metric, essentially treating this as a classification problem where the goal was to classify the batch from the target encoder corresponding to the predictor output. A sound method, which was truly impressing at first, but... It didn't work. At this point I was about to quit, since nothing seemed to work. As I final struggle I threw all at Deepseek no matter the result, it would tweak some things here and there, but the results did not change, in fact it worsened. 

University is starting soon and I am running out of time, pressured by time and frustration, instead of doing proper research, I stopped coding and went on to let GLM lecture me about JEPA. There I found out about VICReg and hoplefully I will see if this approach finally leads us to a model capable of learning meaningful information.
***
**My Name is Einar and I will be back real soon with great news.
Until then, may your skies be Blue and your winds be low.**
***

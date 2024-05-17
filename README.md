# CS4782-LoRA-ReImplementation

## Introduction

Our project aims to implement Low Rank Adaption of Large Language Models, a new method in training pre trained LLMs for a downstream task. 

The paper we are replicating is LoRA: Low Rank Adaption of Large Lanugage Models. The authors are: Edward Hu, Yelong Shen, Phillip Wallis, Zeyuan Allen-Zhu, Yuanzhi Li, Shean Wang, Lu Wang, Weizhu Chen.

LoRA proposes an alternate method to adapt a model to downstream tasks. LoRA freezes all the pretrained weights of the model and injects trainable weight matrices of low rank into the transformer. Through this we are able to reduce the necessary trainable parameters from d x d, to d x r + r x d. Depending on how many and where LoRA weights are added, the number of trainable parameters can be reduced by a factor of 10,000.

The main contribution of this proposed method is that the lower number of trainable parameters makes adapting pre-trained LLMs easier and more convenient. When using LoRA there is no longer a need to store and update each of the millions and billions parameters that are present in modern LLMs. This greatly reduces the necessary storage, hardware requirement, and training time. In addition, LoRA adaption even proves to be more accurate than normal fine tuning in both natural language understanding and natural language generation.

## Chosen Result

We chose to reproduce the results of LoRA on natural language generation. The specific results are when LoRA was applied to GPT-2 Medium models and tested against the E2E NLG Challenge dataset. We decided to focus solely on the metrics provided when GPT2-M was fine tuned compared to when GPT2-M was trained with LoRA.

Include link to pics

We chose these results as they represented how the LoRA method not only was able to massively decrease the number of trainable parameters, but was also able to achieve higher accuracy scores when compared with normal fine tuning.

## Re-implementation Details




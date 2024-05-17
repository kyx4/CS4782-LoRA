# CS4782-LoRA-ReImplementation

## Introduction

Our project aims to implement Low Rank Adaption of Large Language Models, a new method in training pre trained LLMs for a downstream task. 

The paper we are replicating is LoRA: Low Rank Adaption of Large Lanugage Models. The authors are: Edward Hu, Yelong Shen, Phillip Wallis, Zeyuan Allen-Zhu, Yuanzhi Li, Shean Wang, Lu Wang, Weizhu Chen.

LoRA proposes an alternate method to adapt a model to downstream tasks. LoRA freezes all the pretrained weights of the model and injects trainable weight matrices of low rank into the transformer. Through this we are able to reduce the necessary trainable parameters for a layer from d x d, to d x r + r x d, where r can be as low as a single digit. Depending on how many and where LoRA weights are added, the number of trainable parameters can be reduced by a factor of 10,000.

The main contribution of this proposed method is that the lower number of trainable parameters makes adapting pre-trained LLMs easier and more convenient. When using LoRA there is no longer a need to store and update each of the millions and billions parameters that are present in modern LLMs. This greatly reduces the necessary storage, hardware requirement, and training time. In addition, LoRA adaption even proves to be more accurate than normal fine tuning in both natural language understanding and natural language generation.

## Chosen Result

We chose to reproduce the results of LoRA on natural language generation. The specific results are when LoRA was applied to GPT-2 Medium models and tested against the E2E NLG Challenge dataset. We decided to focus solely on the metrics provided when GPT2-M was fine tuned compared to when GPT2-M was trained with LoRA.

[Paper results](results/paper_results.PNG)

We chose these results as they represented how the LoRA method not only was able to massively decrease the number of trainable parameters, but was also able to achieve higher accuracy scores when compared with normal fine tuning.

## Re-implementation Details

We followed the methodology used in the paper and applied injected LoRA matrices to the Wq and Wv matrices of each attention layer within GPT2-M. Following the paper, we used rank 4 matrices. To implement this, we used HuggingFace to retrieve the pre-trained GPT2-M model. 

The dataset we used was the E2E NLG Challenge Dataset. This consisted of meaning representations, consisting of facts and details about a resturant/cafe, and human references that contain the details presented in normal human language. As GPT2-M is a causal and autoregressive model and not a seq2seq model, we converted the dataset to only contain inputs of [meaning representation = human reference] 

We used HuggingFace's Trainer class to train both a LoRA variant GPT2-M and finetune a normal GPT2-M model. 

To evaluate the results we used 5 different metrics: BLEU, NIST, MET, ROUGE-L, and CIDEr. All of which compare a given sentence and calculate how similar/accurate it is given reference sentences.

In total, training LoRA model and finetuning took over 5 and a half hours of training on a single P100 GPU, with the GPU VAM usage peaking at 8.9 GB for fine tuning.

All libraries are present within the notebook, to replicate our results it is necessary to just run all of the cells.

## Results and Analysis

[Model Results](/results/model_results.PNG)

One discrepancy between our results and the paper’s was that the paper had approximately 0.04 M less parameters. This may be due to some unlisted detail about their implementation as we followed exactly the steps to create the LoRA matrices that were outlined by the paper.

Another discrepancy was that the paper’s implementation of Fine-Tuning and LoRA performed better than our implementation on most metrics. This is most likely due to either the above difference in number of parameters or differences in preprocessing. The E2E NLG dataset consisted of a meaning representation and a human reference, and while we combined this as training input in the format meaning representation = human reference, it is unclear how the paper preprocessed the input.

Despite the discrepancies, the results we reproduced supported the potential of LoRA in lowering computational requirements [(~40% reduction in training time and ~20% reduction in GPU VRAM usage)](results/model_runtime.PNG) and the number of parameters greatly while still having good or better performance than fine-tuning. This was one of the paper’s main contributions as it lowers the barrier of entry, and also makes fine tuning easier for LLM’s with an increasing number of parameters that are updated every few months.

## Conclusion and Future Work

The re-implementation effort gave us insight into the process of fine tuning a pre-existing model, and by comparing the results of fine-tuning and LoRA we were able to see firsthand the effects of LoRA in lowering the computational requirement. This could potentially be applied in the future by allowing us and other private individuals with less computing power to fine-tune LLM’s with a larger number of parameters, such as Llama.

The challenges we faced during the implementation also gave us more insight into implementing specific types of transformers. For example, GPT-2 being a CausalLM meant that we specifically needed to use a pad token rather than the default of setting the pad token to the eos token in Seq2Seq transformers, as this could lead to the model ignoring the eos token.

## References

Original Paper: LoRA: Low Rank Adaption of Large Lanugage Models by Edward Hu, Yelong Shen, Phillip Wallis, Zeyuan Allen-Zhu, Yuanzhi Li, Shean Wang, Lu Wang, Weizhu Chen. [https://arxiv.org/abs/2106.09685](https://arxiv.org/abs/2106.09685)

Metrics: The metrics BLEU, NIST, METEOR, ROUGE-L, CIDEr: [https://github.com/tuetschek/e2e-metrics](https://github.com/tuetschek/e2e-metrics)

Dataset: 

  The E2E Dataset: New Challenges for End-to-End Generation, by Novikova, Jekaterina and Duvsek, Ondrej and Rieser, Verena, at Proceedings of the 18th Annual Meeting of   the Special Interest Group on Discourse and Dialogue, from Saarbrucken, Germany, 2017
  [https://arxiv.org/abs/1706.09254](https://arxiv.org/abs/1706.09254)






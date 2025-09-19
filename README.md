we introduce ScaleQuest, a scalable and novel data synthesis method that utilizes ``small-size'' (e.g., 7B) open-source models to generate questions from scratch without the need for seed data with complex augmentation constraints.



### Method Overview

![](/static/images/method.png)

1. **Question Fine-Tuning (QFT)**: To activate the model’s question generation capability, we first perform Question Fine-Tuning (QFT), where we train the problem-solving model using a small set of problems. We train two question generators based on DeepSeekMath-7B-RL and Qwen2-Math-7B-Instruct, using 15K problems from the GSM8K and MATH training set.
2. **Question Preference Optimization (QPO)**: We further optimize the two problem generators through preference tuning, focusing primarily on two aspects: problem solvability and difficulty.
3. **Question Filtering**: After the QFT and QPO phases, we obtained two question generators: DeepSeekMath-QGen and Qwen2-Math-QGen. However, there were still some minor issues in the generated questions, so we applied filtering methods, including language filtering, solvability filtering, and difficulty sampling.
4. **Response Generation & Reward Filtering**: We generate responses using Qwen2-Math-7B-Instruct and use the reward model score as a metric for evaluating response quality. The response with the highest reward score among the five candidates is then selected as the final response.

![](/static/images/dataset1.png)



### Dataset

With the efficient ScaleQuest, we automatically constructed a mathematical reasoning dataset consisting of 1 million problem-solution pairs, which are more effective than existing open-sourced datasets.

- ScaleQuest-Math-1M: [huggingface link](https://huggingface.co/datasets/dyyyyyyyy/ScaleQuest-Math).

- ScaleQuest-Code: under progress



### Performance of  ScaleQuest

It can universally increase the performance of mainstream open-source models (i.e., Mistral, Llama3, DeepSeekMath, and Qwen2-Math) by achieving 29.2% to 46.4% gains on MATH.
Notably, simply fine-tuning the Qwen2-Math-7B-Base model with our dataset can even surpass Qwen2-Math-7B-Instruct, a strong and well-aligned model on closed-source data, and proprietary models such as GPT-4-Turbo and Claude-3.5 Sonnet.

![](/static/images/results1.png)



![](/static/images/results2.png)



### Cost

The data synthesis process was conducted on a server with 8 A100-40G-PCIe GPUs. Generating 1 million data samples required only 522.9 GPU hours (approximately 2.7 days on an 8-GPU server), with an estimated cost of \$680.8 for cloud server rental. This is only about 10\% of the cost of generating the same data using GPT-4o. This demonstrates that our data generation method is significantly more cost-effective.

![](/static/images/cost.png)

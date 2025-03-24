---
title: "Arabic Leaderboards: Introducing Arabic Instruction Following, Updating AraGen, and More"
thumbnail: /blog/assets/leaderboards-on-the-hub/thumbnail_3c3h_aragen.png
authors:
- user: alielfilali01
  guest: true
  org: inceptionai
- user: SarahAlBarri
  guest: true
  org: inceptionai
- user: Arwa88
  guest: true
  org: inceptionai
- user: samta-kamboj
  guest: true
  org: inceptionai
- user: neha1710
  guest: true
  org: inceptionai
- user: preslavnakov
  guest: true
  org: MBZUAI
---

# Arabic Leaderboards: Introducing Arabic Instruction Following, Updating AraGen, and More

In this blog post, we introduce recent advancements in benchmarking and evaluating large language models within Arabic-language contexts. In collaboration with Mohammed bin Zayed University of Artificial Intelligence (MBZUAI), we present significant updates to our AraGen benchmark and evaluation framework. Key contributions include the public release of the AraGen-12-24 benchmark and associated model answers judged using the 3C3H evaluation metric, the open-sourcing of our entire evaluation codebase, and the establishment of a unified Arabic-Leaderboards Space designed as a central hub for diverse Arabic language model evaluations. 

Additionally, we introduce Arabic Instruction Following (AIF), the first publicly available dataset specifically designed to evaluate language models' capabilities in instruction following within Arabic contexts. The Arabic Instruction Following dataset features samples meticulously crafted to reflect unique linguistic attributes, such as diacritization and distinctive phonetic features. This dataset includes both original, linguistically authentic examples created by our expert linguistic team and carefully adapted samples from English datasets, thoroughly reviewed for cultural relevance. Our comprehensive evaluation framework distinguishes between explicit instructions, directly stated in prompts, and implicit instructions inferred from contextual cues.This release comes accompanied by an open-source evaluation code and a publicly accessible leaderboard as part of the Arabic-Leaderboard Space, benchmarking performance in Arabic and English, in order to provide and easier comparative study of models performance in Instruction Following as a task.

Our work emphasizes transparency, reproducibility, and community-driven improvements, aiming to foster rigorous research practices and robust assessment standards within the Arabic NLP community.

<script type="module" src="https://gradio.s3-us-west-2.amazonaws.com/4.4.0/gradio.js"> </script>
<gradio-app theme_mode="dark" space="inceptionai/AraGen-Leaderboard"></gradio-app>

## About the AraGen-12-24 Release

In December 2024, we introduced the AraGen Benchmark as the foundation for the AraGen Leaderboard. An evaluation framework powered by 3C3H as a metric and adopts a dynamic evaluation philosophy designed to mitigate data contamination and laundering while keeping pace with model improvements. 

Delivering on that promise, today, we publically release the AraGen Benchmark, 12-2024 version, accompanied with all the models answers being judged by claude-3.5-sonnet following the 3C3H guidelines. We hope this benchmark and models answers release, will get the community to revise them carefully which might even help us spot any sort of odd behaviour that we might have missed and we can fix in our upcoming releases.

<iframe
  src="https://huggingface.co/datasets/inceptionai/AraGen/embed/viewer/default/test"
  frameborder="0"
  width="100%"
  height="560px"
></iframe>

In addition, we have open-sourced our entire code base. This step is intended to enhance transparency and reproducibility, and we welcome community contributions, whether through issue reports or pull requests.

## What’s New Today?

Today’s update marks a shift toward a more comprehensive and unified space for all Arabic evaluations and tasks. The new Arabic-Leaderboards Space is meant to serve as a central hub covering a broad spectrum of evaluations—from language models and (soon) vision-language models to embedding models, tokenizers, and more in the future. We invite interested contributors to reach out to us through the community tab or directly in order to discuss how to integrate their work/leaderboards as addiotional tabs into this space.


### AraGen-03-25 Update

In this latest AraGen release, we have expanded the dataset to include 340 pairs of questions and answers, up from 279 in the previous version. The distribution remains relatively similar:
- **Question Answering:** ~200 pairs  
- **Reasoning:** 70 pairs  
- **Safety Questions:** 40 pairs  
- **Orthographic and Grammatical Analysis:** 30 pairs  

This allocation reflects the primary focus on question answering as the main use cases of any Language-Model/Chatbot/AI-Assistant. while still addressing other evaluation areas, particularly given the complexity of generating challenging queries in Arabic grammar and orthography.

![Tasks Distribution (%)](https://huggingface.co/spaces/inceptionai/Arabic-Leaderboards/raw/main/assets/pictures/PercentageDistributionOfTasks.png)

Additionally, we refined the judge system prompt to enhance clarity—even for smaller/weaker judge models. Both the previous and current system prompts are available in our code base, and we encourage the community to experiment more around altering the system prompt for different judges.


#### Dynamic Evaluation and Ranking Analysis

Adopting dynamic evaluation cycles introduces the challenge of ensuring that our benchmark and evaluation pipeline remain consistent and reliable across releases. To illustrate this, we analyze the ranking differences among the top 10 models across various dataset versions and system prompt configurations.

##### Analysis of Ranking Changes

We analyzed model performance under two evaluation scenarios: (1) altering only the system prompt (SP1 vs. SP2) using the latest AraGen version (AraGen-03-25), and (2) updating both the dataset and judge system prompt. The overall rankings were stable, with the top-performing model (*o1-2024-12-17*) consistently maintaining its lead. Notably, we observed a swap in rankings between two Claude models, underscoring the sensitivity of our evaluation approach, especially given their initially close scores.

The only significant change in rankings was for the *gpt-4o-2024-08-06* model, whose performance markedly improved with the updated dataset and prompt. This sudden jump is currently under investigation as part of our ongoing benchmarks-design research.

No major variations occurred solely due to changes in the system prompt, indicating good reproducibility as long as the same judge model (*claude-3.5-sonnet*) is used. However, we anticipate potential variations with smaller or weaker models as judges, where employing the second system prompt (SP2) may enhance consistency.

The robust, consistently top-ranking performance of *o1-2024-12-17* reinforces its reliability for Arabic applications. While recent updates to the evaluation pipeline introduced minor ranking shifts, the overall framework remained stable, with top and bottom performers showing consistency. Many observed ranking adjustments likely reflect typical evaluation error margins due to minor score differences. Notably, the second-ranked model’s score significantly dropped from 78.74% (AraGen-12-24) to 57.38% (AraGen-03-25), clearly indicating that the updated AraGen dataset poses a more challenging benchmark aligned with current advancements in reasoning models.

<details>
  <summary>More Detailed Scores</summary>

###### Couple 1: System Prompt Effect (AraGen-03-25 SP1 vs. AraGen-03-25 SP2)

**Table 1. AraGen-03-25 (SP1) Rankings**

| **Rank** | Model Name                 | 3C3H Score | Correctness | Completeness | Conciseness | Helpfulness | Honesty | Harmlessness |
|----------|----------------------------|------------|-------------|--------------|-------------|-------------|---------|--------------|
| 1        | o1-2024-12-17              | 69.49%     | 74.90%      | 73.04%       | 47.11%      | 72.40%      | 74.56%  | 74.90%       |
| 2        | gpt-4o-2024-08-06          | 56.10%     | 61.96%      | 58.92%       | 34.22%      | 58.80%      | 60.81%  | 61.89%       |
| 3        | claude-3-5-sonnet-20241022 | 54.29%     | 59.31%      | 57.65%       | 34.31%      | 57.13%      | 58.01%  | 59.31%       |
| 4        | claude-3-7-sonnet-20250219 | 53.21%     | 59.31%      | 56.76%       | 28.53%      | 56.86%      | 58.53%  | 59.24%       |
| 5        | o3-mini-2025-01-31         | 51.65%     | 56.67%      | 54.31%       | 31.74%      | 54.46%      | 56.10%  | 56.59%       |
| 6        | deepseek-chat              | 47.82%     | 54.31%      | 52.35%       | 20.56%      | 51.94%      | 53.46%  | 54.31%       |
| 7        | claude-3-5-haiku-20241022  | 43.62%     | 48.14%      | 44.61%       | 28.92%      | 45.37%      | 46.57%  | 48.14%       |
| 8        | o1-mini-2024-09-12         | 43.60%     | 47.55%      | 47.06%       | 26.54%      | 46.35%      | 46.57%  | 47.55%       |
| 9        | Qwen/Qwen2.5-72B-Instruct  | 42.18%     | 48.63%      | 47.55%       | 16.03%      | 44.93%      | 47.38%  | 48.55%       |
| 10       | gpt-4o-mini-2024-07-18     | 40.96%     | 45.10%      | 44.02%       | 24.24%      | 43.19%      | 44.14%  | 45.10%       |

**Table 2. AraGen-03-25 (SP2) Rankings**

| **Rank** | Model Name                 | 3C3H Score | Correctness | Completeness | Conciseness | Helpfulness | Honesty | Harmlessness |
|----------|----------------------------|------------|-------------|--------------|-------------|-------------|---------|--------------|
| 1        | o1-2024-12-17              | 70.25%     | 75.88%      | 70.98%       | 51.25%      | 72.55%      | 75.25%  | 75.59%       |
| 2        | gpt-4o-2024-08-06          | 57.38%     | 63.14%      | 56.67%       | 39.95%      | 59.66%      | 61.79%  | 63.06%       |
| 3        | claude-3-7-sonnet-20250219 | 56.54%     | 62.25%      | 58.53%       | 34.49%      | 60.39%      | 61.40%  | 62.18%       |
| 4        | claude-3-5-sonnet-20241022 | 55.60%     | 60.49%      | 56.67%       | 39.14%      | 58.60%      | 58.50%  | 60.20%       |
| 5        | o3-mini-2025-01-31         | 51.63%     | 56.08%      | 52.35%       | 36.72%      | 53.53%      | 55.10%  | 56.00%       |
| 6        | deepseek-chat              | 51.00%     | 57.55%      | 53.92%       | 25.61%      | 54.95%      | 56.42%  | 57.55%       |
| 7        | claude-3-5-haiku-20241022  | 44.79%     | 48.92%      | 44.51%       | 32.40%      | 46.67%      | 47.38%  | 48.85%       |
| 8        | o1-mini-2024-09-12         | 43.78%     | 47.55%      | 46.76%       | 28.04%      | 46.27%      | 46.67%  | 47.40%       |
| 9        | Qwen/Qwen2.5-72B-Instruct  | 43.09%     | 48.82%      | 47.55%       | 19.73%      | 46.59%      | 47.11%  | 48.75%       |
| 10       | gpt-4o-mini-2024-07-18     | 40.62%     | 45.10%      | 40.88%       | 27.60%      | 42.06%      | 43.58%  | 44.51%       |

**Observations :**
- **Top and Bottom Stability:** The top two models (o1-2024-12-17 and gpt-4o-2024-08-06) remain consistent, as do the lowest-ranked models.
- **Swap Among Claude Models:** A reversal in ranking between claude-3-5-sonnet-20241022 and claude-3-7-sonnet-20250219 indicates that the updated system prompt in SP2 provides a slight advantage to claude-3-7-sonnet.
- **Minor Adjustments:** Overall, apart from the Claude swap, score changes are minimal, suggesting robust evaluation under both prompt conditions.

###### Couple 2: Dataset and Prompt Update Effect (AraGen-12-24 SP1 (old) vs. AraGen-03-25 SP2 (new))

**Table 3. AraGen-12-24 (SP1) Rankings**

| **Rank** | Model Name                     | 3C3H Score | Correctness | Completeness | Conciseness | Helpfulness | Honesty | Harmlessness |
|----------|--------------------------------|------------|-------------|--------------|-------------|-------------|---------|--------------|
| 1        | o1-2024-12-17                  | 82.67%     | 92.71%      | 92.47%       | 34.65%      | 91.19%      | 92.26%  | 92.71%       |
| 2        | claude-3-5-sonnet-20241022     | 78.74%     | 88.31%      | 87.81%       | 33.27%      | 86.97%      | 87.78%  | 88.31%       |
| 3        | claude-3-7-sonnet-20250219     | 77.71%     | 87.89%      | 87.77%       | 29.20%      | 86.27%      | 87.26%  | 87.89%       |
| 4        | gpt-4o-2024-08-06              | 73.89%     | 83.75%      | 82.91%       | 28.94%      | 80.99%      | 83.00%  | 83.75%       |
| 5        | deepseek-chat                  | 71.28%     | 81.89%      | 81.89%       | 21.13%      | 79.53%      | 81.32%  | 81.89%       |
| 6        | o3-mini-2025-01-31             | 70.91%     | 80.29%      | 79.21%       | 27.33%      | 78.38%      | 79.99%  | 80.29%       |
| 7        | claude-3-5-haiku-20241022      | 66.40%     | 74.43%      | 73.36%       | 30.56%      | 72.34%      | 73.30%  | 74.43%       |
| 8        | o1-mini-2024-09-12             | 64.95%     | 74.22%      | 74.22%       | 21.46%      | 72.24%      | 73.32%  | 74.22%       |
| 9        | gpt-4o-mini-2024-07-18         | 63.40%     | 72.10%      | 71.38%       | 22.98%      | 70.41%      | 71.41%  | 72.10%       |
| 10       | Qwen/Qwen2.5-72B-Instruct      | 62.58%     | 71.92%      | 71.80%       | 19.06%      | 69.86%      | 70.94%  | 71.92%       |

**Table 4. AraGen-03-25 (SP2) Rankings**

| **Rank** | Model Name                 | 3C3H Score | Correctness | Completeness | Conciseness | Helpfulness | Honesty | Harmlessness |
|----------|----------------------------|------------|-------------|--------------|-------------|-------------|---------|--------------|
| 1        | o1-2024-12-17              | 70.25%     | 75.88%      | 70.98%       | 51.25%      | 72.55%      | 75.25%  | 75.59%       |
| 2        | gpt-4o-2024-08-06          | 57.38%     | 63.14%      | 56.67%       | 39.95%      | 59.66%      | 61.79%  | 63.06%       |
| 3        | claude-3-7-sonnet-20250219 | 56.54%     | 62.25%      | 58.53%       | 34.49%      | 60.39%      | 61.40%  | 62.18%       |
| 4        | claude-3-5-sonnet-20241022 | 55.60%     | 60.49%      | 56.67%       | 39.14%      | 58.60%      | 58.50%  | 60.20%       |
| 5        | o3-mini-2025-01-31         | 51.63%     | 56.08%      | 52.35%       | 36.72%      | 53.53%      | 55.10%  | 56.00%       |
| 6        | deepseek-chat              | 51.00%     | 57.55%      | 53.92%       | 25.61%      | 54.95%      | 56.42%  | 57.55%       |
| 7        | claude-3-5-haiku-20241022  | 44.79%     | 48.92%      | 44.51%       | 32.40%      | 46.67%      | 47.38%  | 48.85%       |
| 8        | o1-mini-2024-09-12         | 43.78%     | 47.55%      | 46.76%       | 28.04%      | 46.27%      | 46.67%  | 47.40%       |
| 9        | Qwen/Qwen2.5-72B-Instruct  | 43.09%     | 48.82%      | 47.55%       | 19.73%      | 46.59%      | 47.11%  | 48.75%       |
| 10       | gpt-4o-mini-2024-07-18     | 40.62%     | 45.10%      | 40.88%       | 27.60%      | 42.06%      | 43.58%  | 44.51%       |

**Observations :**
- **Top Model Consistency:** The model o1-2024-12-17 remains at the top in both AraGen-12-24 and AraGen-03-25.
- **Notable Shifts:**  
  - *gpt-4o-2024-08-06* improves from rank #4 in AraGen-12-24 to rank #2 in AraGen-03-25, suggesting that the updated dataset favor its performance. 
  - The Claude variants show a slight reordering, mirroring the previous observations.
- **Mid-Tier Adjustments:** Minor ranking shifts are observed among mid-tier models, which may fall within typical evaluation error margins.

</details>


#### Analysis of 3C3H

As part of our December release, we introduced 3C3H as a new evaluation measure of the chat capability of models, aimed at assessing both the factuality and usability of LLMs’ answers. Over the past three months, we have observed some interesting findings, which we share in this section.

One emergent trend is that the various dimensions are almost perfectly correlated. In most cases, correct answers are scored as both highly helpful and harmless, while most models fail to maintain this correlation for the conciseness dimension. This generally reflects the way we train these models today, where increased helpfulness is often rewarded with higher verbosity. This trend has recently caught the attention of the research community, as exemplified by the release of OpenAI’s GPT-4.5 model. According to their use cases section, answers from GPT-4.5 are more concise than those from GPT-4, while still being equally helpful. (comprehensive evaluation of gpt-4.5 will be available soon - post this release)

![HeatMap for o1-2024-12-17](https://huggingface.co/spaces/inceptionai/Arabic-Leaderboards/raw/main/assets/pictures/o1-heatmap.png)

A model that stood out in this analysis is “silma-ai/SILMA-9B-Instruct-v1.0”, which exhibited a higher conciseness score compared to other open-weight models—even those with larger sizes. However, this gain in conciseness came at the cost of helpfulness and other dimensions when compared to its base model, “google/gemma-2-9b-it”. We believe that this analysis, along with optimizing for 3C3H, will enable the community to develop better models through curated datasets while maintaining the correlation across all dimensions.

![SILMA-9B-Instruct-v1.0 VS Gemma-2-9b-it HeatMaps](https://huggingface.co/spaces/inceptionai/Arabic-Leaderboards/raw/main/assets/pictures/silma-vs-gemma-heatmap.png)

This is an ongoing effort to better understand how these dimensions are interconnected and how various scenarios and training recipes affect this relationship. Below, we provide a space where you can generate heatmaps for any combination of models of your choice. We hope the community finds it helpful in spotting additional trends that we may not have noticed. Ultimately, we aim for this tool to foster more discussion about evaluation and 3C3H, serving as a resource for others’ work.

<script type="module" src="https://gradio.s3-us-west-2.amazonaws.com/4.4.0/gradio.js"> </script>
<gradio-app theme_mode="dark" space="inceptionai/3C3H-HeatMap"></gradio-app>

We believe that one limitation of this analysis is the zeroing rule, whereby we do not evaluate the other dimensions if the answer is not correct. In the future, we plan to investigate further whether an answer can be helpful despite being incorrect, and how dimensions such as conciseness and harmlessness factor into this evaluation if the answer is not correct.


### Instruction Following

Instruction-following capability is a core measure of LLMs performance, crucial for building trustworthy and reliable AI assistants. While Google's original IFEval benchmark effectively assessed English-language models, Arabic remained underserved. To address this gap, we've developed Arabic IFEval, the first publicly available dataset specifically tailored to evaluate Arabic LLMs’ instruction-following capabilities. You could find the dataset on our huggingface link: inceptionai/Arabic_IFEval 


Our Arabic IFEval dataset began by carefully adapting approximately 300 prompts from the original English IFEval. This wasn't a straightforward translation; instead, we thoughtfully adjusted prompts to reflect Arabic linguistic nuances and cultural contexts. Additionally, we created unique Arabic-specific samples from scratch, emphasizing features such as distinctive Arabic phonetics, diacritization, and the targeted use of specific Arabic consonants. These original prompts are designed to test model capabilities unique to Arabic language usage, going beyond what English benchmarks typically capture.


To rigorously evaluate models, we employed a detailed methodology combining explicit and implicit evaluation methods. Explicit evaluation used automated scripts to objectively verify if instructions were strictly followed, such as correct formatting or specific word usage. Implicit evaluation tackled subtler linguistic expectations, like maintaining the response language or avoiding repetitive patterns. We also used the scoring metrics at both prompt-level and instruction-level granularity that was introduced by English IFEval, we measured it using strict criteria accuracy which requires perfect adherence to the instruction.


Evaluations of various LLMs highlighted specific challenges unique to Arabic, including language mismatches, unintended safety-blocking behaviors, and frequent hallucinations in outputs. Notably, models performed significantly worse on Arabic prompts compared to their English counterparts, demonstrating the urgent need for benchmarks like Arabic IFEval. To further support research, we have open-sourced the dataset, evaluation scripts, and established a leaderboard that benchmarks more than 40 models, inviting ongoing collaboration and contributions from the NLP community.

## Upcoming Work

As part of our work we will be adding and updating more leaderboards to the Arabic-Leaderboards Space as our internall work progress.
In the upcoming releases, we expect to put online a leaderbaord for visual question-answering across multiple tasks powered by camel-bench and kitab from our collaborators at MBZUAI.
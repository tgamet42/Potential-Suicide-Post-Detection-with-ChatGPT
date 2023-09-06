# Potential-Suicide-Post-Detection-with-ChatGPT
This code uses LangChain, Chromadb, and ChatGPT to detect potential suicide posts in https://www.kaggle.com/datasets/aunanya875/suicidal-tweet-detection-dataset

First and foremost, thank you for sharing this dataset.

Author: Thomas Gamet licensing under the Apache 2.0 license.
The final and best scoring version cost about 0.96 USD per full run through all tweets (more tokens were used than in the original version).

Updated, 90% accuracy with Chat GPT as an LLM performing diagnosis. 
Cohen's Kappa of 0.78. 
Lesson: ChatGPT hallucinates with conflicting inputs, better results via more specific focus, and prompt design matters. Three minor adjustments were tried, and the result below is consistent for what I've been able to prompt from ChatGPT 3.5 (exact model name is in the code below). 
Both agree on potential suicide post = 541 times. 
Both agree on not suicide post = 1068 times. 
Ground truth says potential suicide post and LLM disagrees = 118 times. 
Ground truth says not a suicide post and LLM disagrees = 58 times. 
Calculated Observed Agreement = 0.9014005602240897 
Calculated Chance Agreement = 0.5430178345848143 
Cohen's Kappa= 0.7842378822676176

Reading https://www.nature.com/articles/s41598-018-25773-2 (Identifying Suicide Ideation and Suicidal Attempts in a Psychiatric Clinical Research Database using Natural Language Processing) "The sets were independently classified by all three authors with good inter-rater agreement as indicated by a Cohen’s kappa of 0.85 for suicide ideation and 0.86 for suicide attempt. (See supplementary material for further details on inter-rater agreement and rules: https://github.com/andreafernandes/NLP_Tools_Development). It seems that 86% agreement may be in a gold standard range. I also took a few cues and updated the prompts.

Added a Cohen Kappa calculation with further optimization of the prompts and loops: 
Both agree on potential suicide post = 459 times. 
Both agree on not suicide post = 1100 times. 
Ground truth says potential suicide post and LLM disagrees = 200 times. 
Ground truth says not a suicide post and LLM disagrees = 26 times. 
Calculated Observed Agreement = 0.8733893557422969 
Calculated Chance Agreement = 0.5597266357523402 
Cohen's Kappa= 0.7124271996920467 (this is mid range for generally accepted significant agreement)

There were a total of 226 errors made relative to ground truth.
Accuracy is at 87.3389355742297 %

The obserer agreement looks good, and Kappa is generally also a good result, even if a bit below the gold standard of the article above. For those not familiar with Cohen's Kappa:https://www.statology.org/cohens-kappa-statistic/

I am not sure where an algorithm might be overfitting, but I'm pretty sure a generalized solution can only be a little more accurate without being unusually too accurate relative to human on human comparisons and those human on human should be the most generalized comparisons.

Experiment: Loaded 80% of the tweets for similarity matches in a stratified way made only a small change in accuracy with the main difference in the false negatives. Experiments:

Originally with the LLM deciding on potential suicide posts alone was 82.5% accuracy
With 80% stratified in the vectorstore the potential suicide post achieved 83.6% accuracy
Loading 80% of the tweets with potential suicide post status shows 84.6% accuracy
A prompt improvement aimed at considering depression and becoming tired of life shows 87.3% accuracy
Updated the prompts to aim at improvements after reading (Identifying Suicide Ideation and Suicidal Attempts in a Psychiatric Clinical Research Database using Natural Language Processing)

The OpenAI ChatGPT LLM heavily seems to favor the probability of saying a large number of ground truth cases for potential suicide posts are not suicide posts (shown as false negatives against the ground truth) even when the vector store is indexed with many of the actual data points providing similarity matches for being potential suicide posts.

The prompts were improved, the output made easier to read when confirming the LLMs performance and the number of retries needed when the LLM produces unusable results. Also, the results are now processed to make sure outputs are valid responses, and exceptoins on dictionary processing are handled (keys are found).

1 Use pip to install langchain and Chroma
2 Load and prepare a dataset for ChatGPT to work upon
3 Ask ChatGPT to analyze batches of 4 tweets for suicide related ideation
4 Report False Positives, False Negatives, and overall accuracy (with listing)
5 Report Cohen's Kappa
6 Load a VectorstoreIndex with all the tweets to ask inter-tweet questions
7 Show a few (3) questions and the answers given

You will need to use your own OpenAI API key or change to a 'no cost' LLM. Hope this proves useful to some viewer. A full run of this notebook, using default setting with ChatGPT 3.5, cost about 0.40 USD per OpenAI's usage information.

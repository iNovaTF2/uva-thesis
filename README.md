# Abstract
Transformers form the backbone of many modern Large Language Models (LLMs), leading to extensive research into their inner workings, such as their attention mechanisms, layer interactions, and the ways they process and generate language. The study of BERT-based architectures, known as Bertology, has made significant strides in uncovering the behaviors and mechanisms of LLMs. In this study, we utilize redundancy metrics to identify redundant attention heads and interpret the self-attention mechanism, a key component of BERT. Focusing on a subset of SuperGLUE tasks and employing the BERT and DistilBERT models, we propose a method for identifying attention heads using redundancy matrices and a heuristic pruning algorithm based on redundancy scores. We then analyze the pruned attention heads by examining their self-attention maps to understand their surface-level behavior. Our findings indicate that up to 30% of BERT's attention heads and 40% of DistilBERT's can be pruned without significantly affecting performance metrics, indicating a high degree of redundancy. Additionally, our experiments demonstrate that pruning just 10% of the attention heads can yield substantial inference time improvements, with some tasks experiencing up to a 50% reduction. Furthermore, we reveal that attention heads exhibit numerous similar attention patterns, which partially accounts for their redundant behavior.

# Installation
To get the code working on your local machine. Follow the following steps:
1. **Clone the repository on your local machine:**
```bat
git clone https://github.com/iNovaTF2/uva-thesis.git
cd uva-thesis
```

2. **Download the necessary dependencies:**
```bat
conda env create -f environment.yml
conda activate uva-thesis-env
```
# Data
The datasets used in the study include BoolQ, RTE and WiC from the SuperGlue dataset, developed by Wang et al. (2019). To inspect the datasets, run the following commands:
```python
from datasets import load_dataset
dataset = load_dataset("boolq") # For BoolQ
dataset = load_dataset("SetFit/rte") # For RTE
dataset = load_dataset("super_glue", "wic") # For WiC
```

# Models
The models used in this study are fine-tuned versions of BERT and DistilBERT on the specific tasks. They can be integrated using one of the model links:
- [BERT fine-tuned on BoolQ](rycecorn/bert-fine-tuned-boolq)
- [BERT fine-tuned on RTE](rycecorn/base-bert-fine-tuned-RTE)
- [BERT fine-tuned on WiC](rycecorn/Bert-fine-tuned-WiC)
- [DistilBERT fine-tuned on BoolQ](rycecorn/distil-bert-fine-tuned-boolq-v2)
- [DistilBERT fine-tuned on RTE](rycecorn/DistilBert-fine-tuned-RTE)
- [DistilBERT fine-tuned on WiC](rycecorn/DistilBert-fine-tuned-WiC)

**Example of loading the fine-tuned BERT model on BoolQ**
```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
tokenizer = AutoTokenizer.from_pretrained(model_link)
model = AutoModelForSequenceClassification.from_pretrained(model_link)
```

# How to recreate the study
The study is divided into two main parts: Pruning and evaluation of the pruned heads

1. **Model pruning:**
Run `1. get_ahr_matrices.ipynb` to generate the Attention Head Redundancy (AHR) matrices. Then run `2. pruning_experiments.ipynb` to start the pruning experiments and retrieve the performance and inference time affected by the different pruning ratios.

2. **CNN training (optional):**
Run `3a.random_select_image_training.ipynb` to randomly sample different attention patterns to train the pattern classifier. The images will need to be hand-labeled as one of the five major self-attention patterns. Once the images have been labeled, run `3b.CNN_training.ipynb` to train the image classifier. Otherwise, use the pre-trained CNN `CNN_classify_attention_patterns.pth` (73% accuracy) for the next steps.

3. **Head analysis:**
Run `4.pattern_analysis.ipynb` to generate the plots for the self-attention pattern of the heads over different pruning ratios and location of the pruned heads relative to its position in the model architecture. 

**Note:** Change the parameters in the beginning of the notebook to the desired task and model. Keep track of the outputs generated by each notebook and update the parameters in the sequential notebooks to match the outputs.
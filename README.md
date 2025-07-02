# Fine-Tuning Documentation
---
### **1. Task Overview**

We fine-tuned a 4-bit quantized **LLaMA 3.1 8B Instruct model** using the [TweetEval Sentiment Analysis dataset](https://huggingface.co/datasets/tweet_eval) and Customizing it. The objective was to classify tweets as **positive, negative, or neutral**.

---

###  **2. Dataset Details**

| Property          | Value                                         |
| ----------------- | --------------------------------------------- |
| Dataset Name      | `tweet_eval`                                  |
| Subset            | `sentiment`                                   |
| Preprocessed Size | 1,000 samples (train) + 200 (validation)      |
| Input Format      | (Instruction, Input, Output) |

#### Input Formatting Example:

```
### Instruction:
Classify the sentiment of the tweet as positive, negative, or neutral.

### Input:
I'm so happy today!

### Output:
positive
```

---

###  **3. Fine-tuning Configuration**

| Parameter                  | Value                                                                |
| -------------------------- | -------------------------------------------------------------------- |
| Model                      | `unsloth/Meta-Llama-3.1-8B-Instruct-bnb-4bit`                        |
| Finetuning Strategy        | LoRA via `unsloth`                                                   |
| LoRA Target Modules        | q\_proj, k\_proj, v\_proj, o\_proj, gate\_proj, up\_proj, down\_proj |
| LoRA Params Trained        | \~42M / 8B (0.52%)                                                   |
| Max Sequence Length        | 2048 tokens                                                          |
| Batch Size                 | 2                                                                    |
| Gradient Accumulation      | 4                                                                    |
| Total Effective Batch Size | 8 (2 × 4)                                                            |
| Max Steps                  | 60                                                                   |
| Learning Rate              | 2e-4                                                                 |

---
### **4. Results**

#### Training Output:

* `Train Loss`: **2.13**
* `Runtime`: \~2432 sec

#### Evaluation Output:

* `Validation Loss`: **1.88**
* `Eval Runtime`: \~587 sec

####  Analysis:

* The **validation loss is lower than training loss**, indicating no overfitting.
* Training loss improved by `0.25`, suggesting meaningful learning occurred.


---

###  **5. Conclusion**

The LLaMA 3.1 8B model successfully fine-tuned on a small sentiment dataset using LoRA and Unsloth with quantized weights.
Performance showed **no signs of overfitting**, and results can be further improved with:

* Larger dataset
* More training steps
* Scheduled learning rate
---
### **6. HuggingFace Model**
[HF_Model_LINK](https://huggingface.co/krtkjais/llama3.1-8B-tweet_eval_sentiment-finetuned)

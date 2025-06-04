# LLMs

> **Module Overview:**
> This module introduces Large Language Models (LLMs). We focus on connecting LLM concepts to supervised learning and reinforcement learning, and highlight practical implications for using LLMs effectively.


---
## References

[Deep Dive into LLMs like ChatGPT @AndrejKarpathy](https://www.youtube.com/watch?v=7xTGNNLPyMI)
[Large Language Models explained briefly @3blue1brown](https://www.youtube.com/watch?v=LPZh9BOjkQs)
[Introduction to Reinforcement Learning and its Role in LLMs](https://huggingface.co/learn/llm-course/en/chapter12/2)

---

## Table of Contents
- [Data Preparation](#data-preparation)
- [Tokenization](#tokenization)
- [Training: Next Token Prediction](#training-next-token-prediction)
- [Inference: Generating Text](#inference-generating-text)
- [Supervised Fine-Tuning](#supervised-fine-tuning)
- [Hallucination](#hallucination)
- [Using tools](#using-tools)
- [Counting and Arithmetic](#counting-and-arithmetic)
- [Reinforcement Learning](#reinforcement-learning)

---

## Data Preparation

The journey of building an LLM begins with data. An example is the [FineWeb dataset](https://huggingface.co/spaces/HuggingFaceFW/blogpost-fineweb-v1), which contains a vast amount of text data.

> **Practical implication:**
> The quality and diversity of your dataset directly affect what the model can learn and how well it generalizes.

## Tokenization

Computers see 0s and 1s. For example, each char is represented by a number in the ASCII table, e.g. 'a' is 97, 'b' is 98, etc.

```python
s = "How are you"
int_sequence = [ord(c) for c in s]
print(int_sequence)
```

"How are you" would be represented as: [72, 111, 119, 32, 97, 114, 101, 32, 121, 111, 117]

Therefore, this huge amount of text data is actually a sequence of numbers.

However, working with raw characters is inefficient. If a sequence of numbers occurs frequently, it is better to represent it with a single "ID" instead of multiple numbers. This process is known as **tokenization**.

For example, the sequence "72, 111, 119" occurs frequently, so it can be represented as a single token, let's say 4438.

You can see how tokenizer works [here](https://tiktokenizer.vercel.app/?model=cl100k_base). (`cl100k_base` is a tokenizer used by OpenAI models.) "How are you" would be represented as a sequence of tokens, such as `[4438, 527, 499]`. As another example, "ubiquitous" is actually broken into 3 tokens `[392, 5118, 5085]`.

The vocabulary is the set of all tokens that the model can understand. For instance, the vocabulary for the `cl100k_base` tokenizer includes around 100,000 tokens.

> **Connection:**
> Tokenization is a form of feature engineering, similar to how you might encode categorical variables in supervised learning.

> **Practical implication:**
> Understanding tokenization you anticipate model limitations (e.g., #counting-and-arithmetic).

## Training: Next Token Prediction


One way to train a LLM is **next token prediction**: the model take a sequence of tokens as input and predicts the next token in the sequence. 


![training](fig_training.png)

**Connection:**
Training an LLM for next token prediction is fundamentally a supervised learning problem, just like classification. The model is given an input sequence (features) and learns to predict the next token (label) from a fixed vocabulary—each token is a possible class. LLMs are parametric models (large neural networks) with millions or billions of parameters. The final layer is a softmax, which outputs probabilities for each possible next token. Training uses gradient descent to minimize the cross-entropy loss between the predicted probabilities and the actual next token, exactly as in standard neural network classification tasks.

> **Practical implication:**
> Understanding this connection helps you see why LLMs can generalize well to new text, but also why they may make mistakes similar to other supervised models (e.g., overfitting, bias from training data).

## Inference: Generating Text

During inference, the model generates text based on a given context window (a fixed-length sequence of previous tokens). 


> **Practical implication:**
This is a stochastic process. Some vendor provide a option "balance", "creative", or "precise" to control the randomness of the output. For "precise", we might prefer to sample the most likely next token, while for "creative", we might sample from a wider range of possibilities.


**Context Window**
The context window size is the maximum number of tokens the model can consider at once. For example, GPT-4 has a context window size of 128k tokens. This is like the working memory of the model.

> **Practical implication:**
> - Starting a new chat resets the context and refresh the model's memory. This is useful for keeping conversations coherent and relevant.
> - You can add files to the context window, such as PDFs or text documents. They are tokenized and included in the context window, allowing the model to reference their content when generating responses. 
> - If you have very long conversation, the model may not be able to remember everything, leading to potential loss of context or coherence in the generated text.

For a visual explanation, see this [video demonstration](https://www.youtube.com/watch?v=wjZofJX0v4M&t=183s).


**Knowledge Cutoff**
LLMs are trained on data up to a certain point in time, known as the knowledge cutoff. This means it does not have information about events or developments that occurred after that date.


> **Practical implication:**
If you ask about recent events, the model may not provide accurate or up-to-date information. See #tool-usage for how to handle this limitation.


## Supervised Fine-Tuning (SFT)

After pretraining, LLMs are often fine-tuned on specific datasets, such as user-assistant conversations. For example, the [OpenAssistant oasst1 dataset](https://huggingface.co/datasets/OpenAssistant/oasst1/viewer/default/train?row=0&views%5B%5D=train) contains dialogues that help models learn to follow instructions and provide helpful responses.


See [example](https://tiktokenizer.vercel.app/?model=gpt-4o) for how to tokenize a conversation. This can be achieved by introducing special tokens like `<|im_start|>` and `<|im_end|>` to denote the beginning and end of user messages.


> **Practical implication:**
Different models went thorough different SFT, leading to different "personalities" or "styles".

**Open Source Models** 
When a model is open source, it usually means that the model weights and architecture are publicly available. But usually, the training data and the training process are not disclosed. 


Hugging Face is a popular platform for sharing and using open-source models. You can also play with models on your local machine using LM Studio. Usually, the pre-trained model is named "base model", and the SFT model is named "instructed model". 


## Hallucination

Pre-trained models can always generate the next token. Even with text like "what happened in 2099", it still tries to complete it can generate a plausible-sounding response, even though it has no knowledge of the future. Early LLMs, like GPT-2, were known for this behavior, which is often referred to as "hallucination."

To mitigate hallucination, one approach is to include "I don't know" in the conversation dataset. This helps the model learn to recognize when it does not have enough information to provide a meaningful answer.


## Using tools

When an LLM needs to answer a question that requires up-to-date or external information, it can use tools to enhance its response. The process works as follows: First, the model detects that the user's query is beyond its knowledge cutoff or requires real-time data, such as current events or calculations. Next, the LLM formulates a structured request for the appropriate tool, such as a web search, database lookup, or calculator. The tool then executes the request and returns the result to the LLM. Finally, the LLM integrates this new information into its context window and uses it to generate a more accurate and informed response for the user.


Here's an example that illustrates this process (performed on 6/2/2025):

```
User: How many times did Oklahoma City Thunder went into NBA finals?

ChatGPT 4o: As of June 2025, the Oklahoma City Thunder have reached the NBA Finals twice since relocating from Seattle in 2008. Their first appearance was in 2012, where they lost to the Miami Heat in five games. Their second appearance is in the ongoing 2025 NBA Finals against the Indiana Pacers, with the series set to begin on June 5. [Link to wikipedia]
```

Here, the model used a web search tool to find the latest information about the Oklahoma City Thunder's NBA Finals appearances, which is beyond its knowledge cutoff.

```
User: How many times did Oklahoma City Thunder went into NBA finals? Don't search. Answer immediately.

ChatGPT 4o: The Oklahoma City Thunder have made one NBA Finals appearance — in 2012, where they lost to the Miami Heat.
```

Here, I instructed the model not to search, therefore the answer is not up-to-date.


> **Practical implication:** 
Once we know how LLM works, we can understand why it sometimes struggles with counting and arithmetic.
> - LLM is doing next token prediction, which is not calculations.
> - Large numbers or long words can be tokenized into multiple tokens. LLMs do not "see" the input as humans do, but as a sequence of tokens that may not align with the intended units (words, numbers, etc.)

But they can use tools like calculators or code interpreters to perform precise calculations. For example, if you ask a model to  "What is 123456789 * 987654321", it might use a calculator tool to provide the correct answer.
But if you say "What is 123456789 * 987654321, answer immediately", the model might struggle to give the correct answer because it is not designed for precise arithmetic calculations.


# Reinforcement Learning


## Alignment

After Pre-training and Supervised Fine-Tuning, we have a useful chatbot that are fluent in conversation.

However, it may still produce harmful or biased responses, or not align with human values. **Reinforcement Learning (RL)** is used to further improve the model's behavior by aligning it with human preferences and values.

Agent: LLM

Environment: The environment provides feedback to the agent based on its actions.

Actions: The model's responses to user queries.

Rewards: Numerical feedback from the environment, indicating how well the model's responses align with human preferences


### Reinforcement Learning from Human Feedback (RLHF)

A very popular technique for aligning language models is Reinforcement Learning from Human Feedback (RLHF). In RLHF, we use human feedback as a proxy for the “reward” signal in RL. Here’s how it works:

Get Human Preferences: We might ask humans to compare different responses generated by the LLM for the same input prompt and tell us which response they prefer. For example, we might show a human two different answers to the question “What is the capital of France?” and ask them “Which answer is better?“.

Train a Reward Model: We use this human preference data to train a separate model called a reward model. This reward model learns to predict what kind of responses humans will prefer. It learns to score responses based on helpfulness, harmlessness, and alignment with human preferences.

Fine-tune the LLM with RL: Now we use the reward model as the environment for our LLM agent. The LLM generates responses (actions), and the reward model scores these responses (provides rewards). In essence, we’re training the LLM to produce text that our reward model (which learned from human preferences) thinks is good.


### Reasoning Model

Another place where RL is useful is in improving the model's reasoning capabilities.

For example, if a model is asked to solve a math problem, it might generate a series of steps leading to the answer. If these steps are correct, the model receives a positive reward;. Over time, the model learns to produce more accurate and logical reasoning paths.

Example of reasoning model/thinking model: [DeepSeek-R1](https://arxiv.org/abs/2501.12948), OpenAI o3 and o4-mini, Gemini-2.5 Pro, etc.


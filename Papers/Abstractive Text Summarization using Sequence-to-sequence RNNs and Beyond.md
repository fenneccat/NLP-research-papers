Authors: Nallapati, Ramesh, et al. (IBM Watson)
Year: 2016
Conference or Journal: CoNLL
Paper: [Abstractive Text Summarization using Sequence-to-sequence RNNs and Beyond](https://www.aclweb.org/anthology/K16-1028.pdf)

## Abstact
* Use Attentional Encoder-Decoder Recurrent Neural Networks
* Overcome limitation of previous models (modeling key-words, capturing the hierarchy of sentence-toword structure, and emitting words that are rare or unseen at training time)

## Introduction
Abstractive text summarization is the task of generating a headline or a short summary consisting of a few sentences that captures the salient ideas of an article or a passage.
* Use sequence-to-sequence model which outperform in Machine Translation (MT) tasks
* However, there are differences compare to MT
  1. Unlike in MT, the target (summary) is typically very short - Length is not dependent to source.
  2. Optimally compress the original document in a lossy manner such that the key concepts in the original document are preserved

### Main Contributions
(i) Apply the off-the-shelf attentional encoder-decoder RNN 
(ii) Propose novel models and show that they provide additional improvement in performance (Not just using MT model structure)
(iii) Propose a new dataset for the task of abstractive summarization of a document into multiple sentences and establish benchmarks
  -  DUC Corpus: [DUC website](https://duc.nist.gov/duc2004/tasks.html)

## Models
![](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_model.jpg)
### Encoder-Decoder RNN with Attention and Large Vocabulary Trick
* Restrict Decoder vocabulary for certain batch to words in the source documents of that batch. + the most frequent words in the
target dictionary are added until the vocabulary reaches a fixed size
  - It leads to avoid using other words that is not shown on source script that they try to summary.
  - Also, resolve computational bottleneck problem (because vocab size is small, softmax computation become easier)
  
### Capturing Keywords using Feature-rich Encoder
* One of the key challenges is to identify the key concepts and key entities in the document
  - They use not only information of word embedding but also use linguistic features such as TF-IDF statistics of words, Named-entity tags, Part-of-speech tags (Check above model image)

### Modeling Rare/Unseen Words using Switching Generator-Pointer
![](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_decoder-pointer-architecture.jpg)
* Since they restric vocabulary (Check **Encoder-Decoder RNN with Attention and Large Vocabulary Trick** section above), Out-of-vocabulary (OOV) Problem is inevitable.
![swich equation](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_switch-equation.jpg)
* They solve this problem by pointer architecture
  - Decoder decide to use whether generator or pointer by turning on or off 'switch' at every time-step
  - `Generator` generates words in normal fashion, and `Pointer` select one of word-positions in the source.
  - `Switch` operated by several factors:
    - $$\\h_i\\$$: Hidden state for decoder at i-th time step
    - $$E[o_{i-1}]$$: Embedding vector of emission from the previous time step
    - $$c_i$$: Attention-weighted context vector
    - W,v,b are parameters to train
  - When the switch is off (have to use pointer), they select word position with below equation
  ![pointer equation](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_pointer-attention.jpg)
    - It looks similar to `switch` equation, but carefully see superscript of W,v,b. They are different parameters from `switch`.
    - By looking at the previous hidden state, embedding of previous step, and *hidden state of encoding hidden state*, they select right position of word to replace OOV
      - If $$P_{i}^{a}(j)$$ is max in j-th position in source document, they select this word to put into next decoder step.
* loss function
![](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_loss-function.jpg)
$$g_{i}$$ is working as compulsory switch for training time. When OOV happens in training time, $$g_{i}$$ is set to zero, so it can learn parameters for pointer architecture


### Capturing Hierarchical Document Structure with Hierarchical Attention
* It is also important to identify the key sentences from which the summary can be drawn.

![](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_attention-for-key-sentence.jpg)
They use two bidirectional attention weight to determine important sentence.
$$P_{w}^{a}(j)$$ is for word-level attention, $$P_{s}^{a}(j)$$ is for sentence-level attention.
They try to find best position (j) among source document by weighting both word itself and sentence which contains that word.
![](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_word-sentence-weighting.jpg)

## Results
![](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_performance-table.jpg)

### Qualitative Result
![](https://github.com/fenneccat/NLP-research-papers/blob/master/Images/Abstractive_Text_Summarization_qualitative-analysis.jpg)


## Conclusion & My Thought
* They apply the attentional encoderdecoder for the task of abstractive summarization with novel approach for summarization
  - Restrict Vocabulary by using words in certain parts
  - Switch `pointer` and `Generator` to deal wtih OOV problem
  - Figuring out key words by considering both words and sentences attention
* I think the biggest limiation of this work is limit of Vocabulary in summarization.
  - Unlike human summarization, it can never use the word outside the source document (except some frequent words in summary documents in training set)
  - Similarly, it is hard to paraphrase the sentence because it basically select the probalble words from source.

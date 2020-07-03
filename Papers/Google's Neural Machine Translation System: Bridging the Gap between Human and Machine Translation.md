Authors: Yonghui Wu, Mike Schuster, et al. (Google)\
Year: 2016\
Conference or Journal: ArXiv\
Paper: [Google’s Neural Machine Translation System: Bridging the Gap between Human and Machine Translation conference](https://arxiv.org/pdf/1609.08144.pdf)

## Abstact
* motivation: 
  1. NMT is computationally expensive in training and in inference
  2. lack robustness, particularly when input sentences contain rare words
=>  GNMT: 
  - structure: a deep LSTM network with 8 encoder and 8 decoder layers using residual connections as well as attention
  - To solve long training time issue: 
    1. Improve parallelism by using attention mechanism which connects the bottom layer of the decoder to the top layer of the encoder
    2. Employ low-precision arithmetic in inference time
  - Handling rare word cases:
    1. Divide words into a limited set of common sub-word units (“wordpieces”)
      => a good balance between the flexibility of “character”-delimited models and the efficiency of “word”-delimited models
  - Other issues:
      - Their beam search technique employs a length-normalization procedure and uses a coverage penalty => over all words in source sentence.
## Introduction
New architecture is suggested (two RNNs) but it has three weaknesses compared to traditional phrase-based translation systems.
  1. slower training & inference speed 
  2. ineffectiveness in dealing with rare words
  3. failure to translate all words

1. They use LSTM RNNs with 8 layers with residual connections between layers
For parallelism, they connect attention to bottom layer of decoder and top layer of encoder
2. * Use sub-word units for inputs and outputs
   * Special treatments to unknown words
3. Beam search technique includes length nomarlization, coverage penalty to cover all input words

GNMT works well in many different languages pairs without language-specific adjustments

- BLEU scores in WMT tasks outperform previous state-of-the-art
- Human evaluations show GNMT reduce translation errors by 60%

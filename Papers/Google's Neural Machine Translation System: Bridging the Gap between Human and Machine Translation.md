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
  

# torch-poet
This is a PyTorch implemention along the ideas of Andrej Karpathy's [char-rnn](https://github.com/karpathy/char-rnn) as described in '[The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)'.

## Overview
This Jupyter notebook trains multi-layer LSTMs on a library of texts and then generates
new text from the neural model. Through color-highlighting, source-references within
the text generated by the model are used to link to the original sources. This visualizes
how similar the generated and original texts are.

## Requirements
* PyTorch (0.1.12)
* Python 3
* Jupyter Notebook

## References
* Andrej Karpathy's [char-rnn](https://github.com/karpathy/char-rnn)
* [The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)
* See [tensor-poet](https://github.com/domschl/tensor-poet) for a similar implementation, using Tensorflow.
* See [rnnreader](https://github.com/domschl/syncognite/tree/master/rnnreader) for a pure C++ implementation of the same idea.

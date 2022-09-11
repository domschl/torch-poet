[![License](http://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat)](LICENSE)
[![alt text][image]][hyperlink]

  [hyperlink]: https://colab.research.google.com/github/domschl/torch-poet/blob/master/torch_poet.ipynb
  [image]: https://img.shields.io/badge/Google%20Colab-Torch%20Poet-yellow.svg (click to start colab)

# torch-poet: a PyTorch char-rnn implementation

This is a PyTorch implemention along the ideas of Andrej Karpathy's [char-rnn](https://github.com/karpathy/char-rnn) as described in '[The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)'.

## Overview

This Jupyter notebook trains multi-layer LSTMs on a library of texts and then generates
new text from the neural model. Through color-highlighting, source-references within
the text generated by the model are used to link to the original sources. This visualizes
how similar the generated and original texts are.

Training and generation can either happen on Google Colab or on a standard local or remote jupyter instance.

## Structure

### 1. Books from Project Gutenberg

The notebook uses the [`ml_indie_tools`](https://github.com/domschl/ml-indie-tools) project to access the Project Gutenberg library for books of given authors, 
languages and/or keywords. A list of matching books is compiled, downloaded, and prepared for the training pipeline. 
On local jupyter installations downloads from Project Gutenberg are cached locally, for Colab Notebooks, the notebook 
will ask to authorize a connection to the user's Google Drive. The Google Drive connection is used to 1. cache 
downloaded text documents and to 2. store training snapshots. 
All activity is only within the `Colab Notebooks/<project name>` and `gutenberg_cache` paths. 
The project name is defined at the beginning of the notebook and serves to differentiate between different training 
configurations and datasets.

Please refer to [`ml_indie_tools`](https://github.com/domschl/ml-indie-tools) for more details.

### 2. Training pipeline

The selected book library is then converted into a Torch `Dataset` that resides on the gpu (if available) and 
is fed into the model using torch `DataLoader`.

### 3. Training

The model can be configured for an arbitrary number of LSTM layers. By default, every 3 minutes, training statistics 
are shown and a training snapshot is generated, and every 10 minutes (as soon as loss is below 1.5), sample text is generated.

Training can be interrupted at any point, and on restart, the last available snapshot is automatically loaded and 
training is continued. This is especially handy for Colab sessions which can be interrupted at any time. Snapshots of Colab trainings reside on the user's Google Drive and are thus persistent over session resets.

### 4. Generation of Text and 'Dialog'

At any point, training can be interrupted, and the snapshot that yielded highest precision can be loaded for text generation.

### Run notebook in Google Colab

<a href="https://colab.research.google.com/github/domschl/torch-poet/blob/master/torch_poet.ipynb"><img src="https://www.tensorflow.org/images/colab_logo_32px.png" height="12" width="12" /> Run notebook in Google Colab</a>

## Requirements to run with standard Jupyter

* PyTorch (1.x)
* Python 3
* Jupyter Notebook

## Performance

Model: 2x256, 64 steps

* Nvidia 1080ti: 0.00011 sec/sample
* Tesla V4 (Colab): 0.00012 sec/sample
* Apple M1 (CPU): 0.0032 sec/sample (Note: at the time of tests there was no pytorch version that supports M1 GPU, hence the very slow result, update 09/2022: while there is now (09/2022) an implementation with GPU support, is crashes during LSTM backward path, so can't be tested, s.b.)

## History

* 2022-09-11: Tested Apple M1 Metal support (MPS), but LSTM backward pass is still [broken](https://github.com/pytorch/pytorch/issues/80306), crashes because getting confused on tensor dimensions. (PyTorch 1.12.1 and nightly of 09/2022 both crash). Latest ml_indie_tools enabled.
* 2022-03-12: [`ml_indie_tools`](https://github.com/domschl/ml-indie-tools) is used for Projekt Gutenberg access.
* ongoing: support for direct Project Gutenberg queries for training data generation, various optimizations,
  usage of torch dataloaders.
* 2020-02-13: PyTorch 1.4, women literature default texts. Saving/Restoring model data
* 2019-05-11: Pytorch 1.1, support for running on Google Colab.
* 2018-10-17: retested with PyTorch 1.0rc1, ok, no changes necessary.
* 2018-05-13: adapted for PyTorch 0.4
* 2018-03-02: adapted for PyTorch 0.3.1

## References

* Andrej Karpathy's [char-rnn](https://github.com/karpathy/char-rnn)
* [The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)
* See [tensor-poet](https://github.com/domschl/tensor-poet) for a similar implementation, using Tensorflow.
* See [rnnreader](https://github.com/domschl/syncognite/tree/master/rnnreader) for a pure C++ implementation of the same idea.
* [`ml_indie_tools`](https://github.com/domschl/ml-indie-tools): Gutenberg access and boiler-plate.
* [transformer-poet](https://github.com/domschl/transformer-poet) for a transformer based implementation (tensorflow).


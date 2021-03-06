[![Build Status](https://travis-ci.org/pyconll/pyconll.svg?branch=master)](https://travis-ci.org/pyconll/pyconll)
[![Coverage Status](https://coveralls.io/repos/github/pyconll/pyconll/badge.svg?branch=master)](https://coveralls.io/github/pyconll/pyconll?branch=master)
[![Documentation Status](https://readthedocs.org/projects/pyconll/badge/?version=stable)](https://pyconll.readthedocs.io/en/latest/?badge=latest)
[![Version](https://img.shields.io/github/v/release/pyconll/pyconll)](https://github.com/pyconll/pyconll/releases)
[![gitter](https://badges.gitter.im/pyconll/pyconll.svg)](https://gitter.im/pyconll/pyconll?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## pyconll

*Easily work with **CoNLL** files using the familiar syntax of **python**.*

##### Links
- [Homepage](https://pyconll.github.io)
- [Documentation](https://pyconll.readthedocs.io/)


### Installation

As with most python packages, simply use `pip` to install from PyPi.

```
pip install pyconll
```

`pyconll` is also available as a conda package on the `pyconll` channel. Only packages 2.2.0 and newer are available on conda at the moment.

```
conda install -c pyconll pyconll
```

This package is designed for, and only tested with python 3.4 and up and will not be backported to python 2.x or to any versions older than python 3.4 as this release has reached end of support starting in 2020. Python 3.4 will remain supported through pyconll version 2.x.


### Support

pyconll fully supports and is regularly tested against all UD v2.x versions to ensure compatibility with the latest releases, and also maintains backwards compatibility. Please feel free to direct any questions to the [gitter channel](https://gitter.im/pyconll/pyconll) or create an issue on GitHub.


### Motivation

This tool is intended to be a **minimal**, **low level**, **expressive** and **pragmatic** library in a widely used programming language. pyconll creates a thin API on top of raw CoNLL annotations that is simple and intuitive.

In my work with the Universal Dependencies project, I saw a dissapointing lack of low level APIs for working with the CoNLL-U format. Most tooling focuses on graph transformations and DSLs for terse, automated changes. Tools such as [Grew](http://grew.fr/) and [Treex](http://ufal.mff.cuni.cz/treex) are very powerful and productive, but their DSLs have a learning curve and limit their scope. [UDAPI](http://udapi.github.io/) offers a python library but it is very large and has little guidance. pyconll attempts to fill the gaps between what other projects have accomplished.

Hopefully, individual researchers find pyconll useful, and will use it as a building block for their tools and projects. pyconll affords an intuitive and complete base for building larger projects without worrying about the details of CoNLL annotation and output.


### Code Snippet

```python
# This snippet finds what lemmas are marked as AUX which is a closed class POS in UD
import pyconll

UD_ENGLISH_TRAIN = './ud/train.conll'

train = pyconll.load_from_file(UD_ENGLISH_TRAIN)

aux_lemmas = set()
for sentence in train:
    for token in sentence:
        if token.upos == 'AUX':
            aux_lemmas.add(token.lemma)
```


### Uses and Limitations

This package edits CoNLL-U annotations. This does not include the annotated text itself. Word forms on Tokens are not editable and Sentence Tokens cannot be reassigned or reordered. `pyconll` focuses on editing CoNLL-U annotation rather than creating it or changing the underlying text that is annotated. If there is interest in this functionality area, please create a github issue for more visibility.

This package also is only validated against the CoNLL-U format. The CoNLL and CoNLL-X format are not supported, but are very similar. I originally intended to support these formats as well, but their format is not as well defined as CoNLL-U so they are not included. Please create an issue for visibility if this feature interests you.

Lastly, linguistic data can often be very large and this package attempts to keep that in mind. pyconll provides methods for creating in memory conll objects along with an iterate only version in case a corpus is too large to store in memory (the size of the memory structure is several times larger than the actual corpus file). The iterate only version can parse upwards of 100,000 words per second on a 16gb ram machine, so for most datasets to be used on a local dev machine, this package will perform well. The 2.2.0 release also improves parse time and memory footprint by about 25%!


### Contributing

Contributions to this project are welcome and encouraged! If you are unsure how to contribute, here is a [guide](https://help.github.com/en/articles/creating-a-pull-request-from-a-fork) from Github explaining the basic workflow. After cloning this repo, please run `make hooks` and `pip install -r requirements.txt` to properly setup locally. `make hooks` setups up a pre-push hook to validate that code matches the default YAPF style. While this is technically optional, it is highly encouraged, and CI builds will fail without proper formatting. `pip install -r requirements.txt` sets up environment dependencies like `yapf`, `twine`, `sphinx`, etc.

For packaging new versions, use setuptools version 24.2.0 or greater for creating the appropriate packaging that recognizes the `python_requires` metadata. Final packaging and release is now done with Github actions so this is less of a concern.


#### README and CHANGELOG

When changing either of these files, please change the Markdown version and run ``make gendocs`` so that the other versions stay in sync.


#### Code Formatting

Code formatting is done automatically on push if githooks are setup properly. The code formatter is [YAPF](https://github.com/google/yapf), and using this ensures that coding style stays consistent over time and between authors. The linter can also be setup and run via `make lint`. If the development environment is not properly setup, then the CI build will fail if code is not formatted properly.

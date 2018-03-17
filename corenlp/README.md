# Stanford CoreNLP

[Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/) provides a rich set of state-of-the-art NLP tools. It can be use via command line interface, a Java API, or a web API. To use CoreNLP from programming languages other than Java it is recommended to use the CoreNLP server with its web API.

# CoreNLP Server

The [CoreNLP server](https://stanfordnlp.github.io/CoreNLP/corenlp-server.html) offers a simple web-based interface and a web API.

## Setting up the CoreNLP Server

Running the CoreNLP server with support for German requires downloading the base CoreNLP package and the German language models.

The official [CoreNLP server documentation](https://stanfordnlp.github.io/CoreNLP/corenlp-server.html) describes all necessary steps to setup and run the server.
However, to make things easier, we'll use a docker image which includes all required libraries and models.

### Docker Image

The [official docker image](https://hub.docker.com/r/motiz88/corenlp/) is outdated (last update end of 2015) and does not support German. Hence, we build our own.

*Build docker image:*

```sh
docker build -t corenlp-german .
```

*Run docker image:*

```sh
docker run -p 9000:9000 --name corenlp corenlp-german
```

Once the CoreNLP server is running it can be accessed via http://localhost:9000.

## CoreNLP Python Wrappers

There is a list of python wrappers on the CoreNLP homepage. Following wrapper have been tested.

### [py-corenlp](https://github.com/smilli/py-corenlp) by Smitha Milli

Small and easy to use wrapper. But only works with Unicode when Python 3 is used. Has not been updated since 2016. (Probably does not support the latest updates of CoreNLP with TokenRegex).

Install:
```sh
pip install pycorenlp
```
Basic Usage:
```python
from pycorenlp import StanfordCoreNLP
nlp = StanfordCoreNLP('http://localhost:9000/')
print(nlp.annotate('Köln is a city in Germany.'))
```

### [stanford-corenlp](https://github.com/Lynten/stanford-corenlp) by Lynten

Annotate() returns the raw json response without parsing. But there are some nice helper functions for pos-tags, ner, etc. However, each call to a helper function sends a new request.

```sh
pip install stanfordcorenlp
```
```python
from stanfordcorenlp import StanfordCoreNLP
nlp = StanfordCoreNLP('http://localhost', port=9000)
print(nlp.annotate('Köln is a city in Germany.'))
```


## Using the CoreNLP Server

### Selecting Text with TokensRegex

[TokensRegex](https://nlp.stanford.edu/software/tokensregex.html) can be used to evaluate regular expressions over text and tokens.

* *Example 1*: Matching measures of thinks, e.g. '15 minutes', as two consecutive tokens, 1st with cardinality type and 2nd is a noun.
        [{pos:CARD}][{pos:NN}]

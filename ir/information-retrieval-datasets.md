# Information Retrieval Datasets

## Background

[Flexible classic and NeurAl Retrieval Toolkit](https://github.com/oaqa/FlexNeuART) published a configurable processing of standard datasets. The source for the datasets are available un [ir-datasets](https://ir-datasets.com).

## Installation

`pip install ir_datasets`

## Usage

```python
import ir_datasets
dset = ir_datasets.load('cord19/trec-covid')

## Documents
[doc for doc in dset.docs_iter()]

## Fields
dset.docs_cls().__annotations__

## Queries
[q for q in dset.queries_iter()]

import pandas as pd
pd.DataFrame(dset.queries_iter())

## Queries Relevance Assessments
pd.DataFrame(dataset.qrels_iter())

# Query Relevance Definitions can be found here
dset.qrels_defs()
```

## Resources

- [Getting Started](https://colab.research.google.com/github/allenai/ir_datasets/blob/master/examples/ir_datasets.ipynb)

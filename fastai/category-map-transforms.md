**Problem**

How to encode the categories of targets in dataframe column or in a numpy array? Also handle scenarios where you may need to add na/abstain column at specified index.
How to map the object2index and reversible mapping using vocabulary from the categories?

**Solution**

This is similar to LabelEncoder class in scikit but using fastai CategoryMap adapted to handle scenarios where I need to add na/abstain column at specified index.

```python
from collections import defaultdict

from fastcore.test import all_equal
from fastcore.foundation import CollBase, L
from pandas.api.types import is_categorical_dtype


# Credit: https://github.com/fastai/fastai/blob/master/fastai/data/transforms.py#L207
# Adapted from fastai transforms
class CategoryMap(CollBase):
    "Collection of categories with the reverse mapping in `o2i`"

    def __init__(self, col, sort=True, add_abstain=True, strict=False):
        if is_categorical_dtype(col):
            items = L(col.cat.categories, use_list=True)
            # Remove non-used categories while keeping order
            if strict:
                items = L(o for o in items if o in col.unique())
        else:
            if not hasattr(col, "unique"):
                col = L(col, use_list=True)
            # `o==o` is the generalized definition of non-NaN used by Pandas
            items = L(o for o in col.unique() if o == o)
            if sort:
                items = items.sorted()
        self.items = "ABSTAIN" + items if add_abstain else items
        self.o2i = (
            defaultdict(int, [(v, k) for k, v in enumerate(self.items, start=-1)])
            if add_abstain
            else dict(self.items.val2idx())
        )

    def map_objs(self, objs):
        "Map `objs` to IDs"
        return L(self.o2i[o] for o in objs)

    def map_ids(self, ids):
        "Map `ids` to objects in vocab"
        return L(self.items[o] for o in ids)

    def __eq__(self, b):
        return all_equal(b, self)
```
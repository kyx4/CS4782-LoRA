#### Data

The data used in the code, although the same was retrieved through HuggingFace.


- trainset is the training split
- devset is the validation split
- testset_w_refs is the test split


To retrieve the dataset from HuggingFace the datasets library was used

Code used to load this dataset
```
from datasets import load_dataset
dataset = load_dataset("e2e_nlg")
```


In order to handle imbalance in the dataset for a text multiclass classification problem, we need to use appropriate weights for our loss function.

fastai provides CrossEntropyLossFlat with the weights as the argument. We can initialize the weights based on the inverse frequency of each category in case of multi class problem.

```
def get_weights(dls):
   
    # 0th index would provide the vocab from text
    # 1st index would provide the vocab from classes
    classes = dls.vocab[1]

    #Get label ids from the dataset using map
    #train_lb_ids = L(map(lambda x: x[1], dls.train_ds))
    # Get the actual labels from the label_ids & the vocab
    #train_lbls = L(map(lambda x: classes[x], train_lb_ids))

    #Combine the above into a single
    train_lbls = L(map(lambda x: classes[x[1]], dls.train_ds))
    label_counter = Counter(train_lbls)
    n_most_common_class = max(label_counter.values()); 
    print(f'Occurrences of the most common class {n_most_common_class}')
    
    # Alternative : Source: https://discuss.pytorch.org/t/what-is-the-weight-values-mean-in-torch-nn-crossentropyloss/11455/9
	# we can use the 1/frequency of category as the weights
    weights = [1/v for k, v in label_counter.items() if v > 0]; return weights 
```

Learner sample code 
```
learn_cls_tsk2 = text_classifier_learner(dls_cls_tsk2, AWD_LSTM, metrics=[accuracy, F1Score(average='macro')]).to_fp16()
learn_cls_tsk2.load_encoder('fine_tuned_enc')
class_weights = torch.FloatTensor(weights).to(dls_cls_tsk2.device)
learn_cls_tsk2.loss_func = CrossEntropyLossFlat(weight=class_weights)
```

## Reference
- [Discord Thread - fastai-help](https://discordapp.com/channels/689892369998676007/748330524556132364/754530946514026518)

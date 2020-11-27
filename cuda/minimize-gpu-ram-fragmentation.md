When you are training a model on GPU, if you face an issue like below

```
RuntimeError: CUDA out of memory. Tried to allocate 142.00 MiB (GPU 0; 11.17 GiB total capacity; 10.25 GiB already allocated; 104.44 MiB free; 10.66 GiB reserved in total by PyTorch)
```
The only way to recover and proceed forward is to __restart the ipython kernel__. 

From fastai docs on [GPU RAM Fragmentation](https://docs.fast.ai/dev/gpu#gpu-ram-fragmentation)
> Given that GPU RAM is a scarce resource, it helps to always try free up anything that’s on CUDA as soon as you’re done using it, and only then move new objects to CUDA. Normally a simple del obj does the trick. However, if your object has circular references in it, it will not be freed despite the del() call, until gc.collect() will not be called by python. And until the latter happens, it’ll still hold the allocated GPU RAM! And that also means that in some situations you may want to call gc.collect() yourself.

Tricks to minimize the gpu memory usage
- `del()` call on the objects once you are done with them
- explicitly call `gc.collect`
- use smaller batch size

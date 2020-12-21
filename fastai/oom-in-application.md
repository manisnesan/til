
# Application facing memory issues

## Issue
Flask application deployed for serving the model predictions for routing is returning 500 Internal Server Error in QA environment but not in any other environments. The application is still running but not serving any requests. 

Python Traceback
```
Dec 11 14:27:25 air-classifier journal: Traceback (most recent call last):

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/flask/app.py", line 2447, in wsgi_app

Dec 11 14:27:25 air-classifier journal:    response = self.full_dispatch_request()

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/flask/app.py", line 1952, in full_dispatch_request

Dec 11 14:27:25 air-classifier journal:    rv = self.handle_user_exception(e)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/flask/app.py", line 1821, in handle_user_exception

Dec 11 14:27:25 air-classifier journal:    reraise(exc_type, exc_value, tb)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/flask/_compat.py", line 39, in reraise

Dec 11 14:27:25 air-classifier journal:    raise value

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/flask/app.py", line 1950, in full_dispatch_request

Dec 11 14:27:25 air-classifier journal:    rv = self.dispatch_request()

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/flask/app.py", line 1936, in dispatch_request

Dec 11 14:27:25 air-classifier journal:    return self.view_functions[rule.endpoint](**req.view_args)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/flasgger/utils.py", line 273, in wrapper

Dec 11 14:27:25 air-classifier journal:    return function(*args, **kwargs)

Dec 11 14:27:25 air-classifier journal:  File "/opt/air-classifier-api/air_service/api.py", line 103, in get_sbr

Dec 11 14:27:25 air-classifier journal:    sbr = calculate_sbr(request_body)

Dec 11 14:27:25 air-classifier journal:  File "/opt/air-classifier-api/air_service/api.py", line 29, in calculate_sbr

Dec 11 14:27:25 air-classifier journal:    predicted_sbr = predict_sbr(case_details=[request_body])

Dec 11 14:27:25 air-classifier journal:  File "/opt/air-classifier-api/sbr_classifier/prediction/predict2.py", line 130, in predict_sbr

Dec 11 14:27:25 air-classifier journal:    ulmfit = predict_ulmfit (case_ulmfit)

Dec 11 14:27:25 air-classifier journal:  File "/opt/air-classifier-api/sbr_classifier/prediction/predict2.py", line 95, in predict_ulmfit

Dec 11 14:27:25 air-classifier journal:    tokenized_df= tokenize_df(df,

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/fastai2/text/core.py", line 217, in tokenize_df

Dec 11 14:27:25 air-classifier journal:    outputs = L(parallel_tokenize(texts, tok_func, rules, n_workers=n_workers, **tok_kwargs)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/fastcore/foundation.py", line 47, in __call__

Dec 11 14:27:25 air-classifier journal:    res = super().__call__(*((x,) + args), **kwargs)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/fastcore/foundation.py", line 318, in __init__

Dec 11 14:27:25 air-classifier journal:    items = list(items) if use_list else _listify(items)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/fastcore/foundation.py", line 254, in _listify

Dec 11 14:27:25 air-classifier journal:    if is_iter(o): return list(o)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/fastai2/torch_core.py", line 735, in parallel_gen

Dec 11 14:27:25 air-classifier journal:    yield from run_procs(f, done, L(batches,idx).zip())

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/site-packages/fastai2/torch_core.py", line 721, in run_procs

Dec 11 14:27:25 air-classifier journal:    for o in processes: o.start()

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/multiprocessing/process.py", line 121, in start

Dec 11 14:27:25 air-classifier journal:    self._popen = self._Popen(self)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/multiprocessing/context.py", line 224, in _Popen

Dec 11 14:27:25 air-classifier journal:    return _default_context.get_context().Process._Popen(process_obj)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/multiprocessing/context.py", line 277, in _Popen

Dec 11 14:27:25 air-classifier journal:    return Popen(process_obj)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/multiprocessing/popen_fork.py", line 19, in __init__

Dec 11 14:27:25 air-classifier journal:    self._launch(process_obj)

Dec 11 14:27:25 air-classifier journal:  File "/opt/app-root/src/miniconda3/envs/sbr/lib/python3.8/multiprocessing/popen_fork.py", line 70, in _launch

Dec 11 14:27:25 air-classifier journal:    self.pid = os.fork()

Dec 11 14:27:25 air-classifier journal: OSError: [Errno 12] Cannot allocate memory
```

## Environment
Python 3.8
Gunicorn with a single worker running in a docker container

$ hostname

`air-classifier.abc.com`

$ head /proc/meminfo
```
MemTotal: 20392044 kB
MemFree: 349208 kB
MemAvailable: 1585944 kB
Buffers: 1036 kB
```

## Root Cause
OSError: [Errno 12] Cannot allocate memory 

## Diagnosis
* Check if we can reproduce the same behavior in other environments
Dev works
```
curl -X POST "http://air-classifier.dev.abc.com/classifier/sbr" -H  "accept: application/json" -H  "Content-Type: application/json" -d "{  \"case_language\": \"en\",  \"case_number\": \"string\",  \"description\": \"Test\",  \"origin\": \"Test\",  \"product\": \"Test\",  \"severity\": \"Test\",  \"subject\": \"Test\",  \"version\": \"Test\"}"
{
  "predicted_sbr": "SysMgmt"
}
```

Stage works
```
curl -X POST "http://air-classifier.stage.abc.com/classifier/sbr" -H  "accept: application/json" -H  "Content-Type: application/json" -d "{  \"case_language\": \"en\",  \"case_number\": \"string\",  \"description\": \"Test\",  \"origin\": \"Test\",  \"product\": \"Test\",  \"severity\": \"Test\",  \"subject\": \"Test\",  \"version\": \"Test\"}"
{
  "predicted_sbr": "SysMgmt"
}
```
* If not check if this is environment specific issue. Determine the memory allocated for the host and also docker specific resource limitation[1].
* If confirmed, this is not an environment issue, check the error log and application startup options and specific behaviors. 

After reviewing the log, this is the line in our application, that indicates that this is performing multiprocessing operations such as forking the process. This means a separate child process is spawned from your current process which will increase the resources such as memory. 
 
```
Dec 11 14:27:25 air-classifier journal:  File "/opt/air-classifier-api/sbr_classifier/prediction/predict2.py", line 95, in predict_ulmfit
Dec 11 14:27:25 air-classifier journal:    tokenized_df= tokenize_df(df,
```

After reviewing the method[2] performing the above operation, it tokenize texts in the dataframe df[text_cols] **in parallel using n_workers** and stores them in df[tok_text_col]. For applications with lower memory, this may be desirable when you have a large dataframe and performing the tokenizations in chunk is a reasonable idea. And infact during training time, when we perform the evaluation on test dataset this is the right approach.

In case of inference time ie during model predictions serving, when you have only a single input, we may not need this tokenization done in parallel and hence we can avoid the multiprocessing entirely. 

Also if you already have multiple models in memory when you are doing ensembled approach, one should completely avoid multiprocessing operations to reduce memory usage. 

## Resolution
* Switch to [tokenize1](https://docs.fast.ai/text.core.html#tokenize1) instead of [tokenize_df]( https://docs.fast.ai/text.core.html#tokenize_df) that performs tokenization in the same process and for single input.

## References

1. [Runtime options with Memory, CPUs, and GPUs](https://docs.docker.com/config/containers/resource_constraints/)
2. https://docs.fast.ai/text.core.html#tokenize_df

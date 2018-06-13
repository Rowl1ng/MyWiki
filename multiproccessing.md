# 多线程编程
## multiproccesing

```python
import multiproccesing

# Try to use the maximum amount of processes if not given.
try:
    nprocesses = nprocesses or multiprocessing.cpu_count()
except NotImplementedError:
    nprocesses = 1
else:
    nprocesses = 1 if nprocesses <= 0 else nprocesses
```

`Pool` object which offers a convenient means of parallelizing the execution of a function across multiple input values, distributing the input data across processes (**data parallelism**). 

```python
pool = multiprocessing.Pool(nprocesses)
# Prepare _fingerprint_worker input
worker_input = zip(filenames_to_fingerprint, [self.limit] * len(filenames_to_fingerprint))

# Send off our tasks
iterator = pool.imap_unordered(_fingerprint_worker, worker_input)
```

### Consuming Python generators

Another common data source that can easily be ingested as a `tf.data.Dataset` is the python generator.

Caution: While this is a convienient approach it has limited portability and scalibility. It must run in the same python process that created the generator, and is still subject to the Python [GIL](https://en.wikipedia.org/wiki/Global_interpreter_lock).

```
def count(stop):
  i = 0
  while i<stop:
    yield i
    i += 1
```

```
  for n in count(5):
    print(n)
```

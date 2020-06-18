# Cloudbutton Toolkit

**The Cloudbutton Toolkit is a Python multicloud library for running serverless jobs**   
It currently supports AWS, IBM Cloud, Google Cloud, Microsoft Azure, Alibaba Aliyun, and more. See [plugins](https://github.com/cloudbutton/cloudbutton/tree/master/docs/backends).

### Quick start
Run functions in the cloud using the [multiprocessing](https://docs.python.org/3.6/library/multiprocessing.html) API:

   
    from cloudbutton.multiprocessing import Pool
    
    def incr(x):
        return x + 1

    pool = Pool()
    res = pool.map(incr, range(10))
    print(res)

Use cloud storage as a filesystem:  

   
    from cloudbutton.multiprocessing import Pool
    from cloudbutton.cloud_proxy import os, open

    filename = 'bar/foo.txt'
    with open(filename, 'w') as f:
        f.write('Hello world!')

    dirname = os.path.dirname(filename)
    print(os.listdir(dirname))

    def read_file(filename):
        with open(filename, 'r') as f:
            return f.read()

    pool = Pool()
    res = pool.apply(read_file, (filename,))
    print(res)

    os.remove(filename)
    print(os.listdir(dirname))

Use remote in-memory cache for fast IPC and synchronization  

   
    from cloudbutton.multiprocessing import Pool, Manager, Lock
    from random import choice

    def count_chars(char, text, record, lock):
        count = text.count(char)
        record[char] = count
        with lock:
            record['total'] += count

    pool = Pool()
    record = Manager().dict()
    lock = Lock()

    # random text
    alphabet = 'abcdefghijklmnopqrstuvwxyz'
    text = ''.join([choice(alphabet) for _ in range(1000)])

    record['total'] = 0
    pool.map(count_chars, [(char, text, record, lock) for char in alphabet])
    print(record.todict())

## Documentation
- [Website](https://cloudbutton.github.io)
- [API Examples](https://github.com/cloudbutton/cloudbutton/tree/master/examples)
- [Toolkit Examples](https://github.com/cloudbutton/examples)

## [Plugins](https://github.com/cloudbutton/cloudbutton/tree/master/docs/backends)

## Use cases
- [Serverless benchmarks](https://cloudbutton.github.io/benchmarks)
- [Moments in Time video prediction](https://cloudbutton.github.io/examples/example_mit)

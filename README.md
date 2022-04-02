# otus-cpython-patches
> Warning: The sources and examples in the repository are only compatible with Python 2.7

## Description
This repository contains the following three patches that add new features 
to the Python language:

1. ``new_opcode.patch``
    This patch adds the new ``LOAD_OTUS`` opcode (Python bytecode instruction, see https://docs.python.org/2.7/library/dis.html). 
    ``LOAD_OTUS`` opcode combines existing ``LOAD_FAST`` and ``LOAD_CONST`` opcodes as follows:

    ```
    LOAD_OTUS c
    ```
    is equivalent to:
    ```
    LOAD_FAST 0
    LOAD_CONST c
    ```
    where ``c`` is the integer argument.
    
    Since the above combination of ``LOAD_FAST`` and ``LOAD_CONST`` is relatively common, this change is intended
    to speed up bytecode execution and make ``dis.dis()`` output a little more compact.

2. ``until.patch`` adds an ``until`` loop operator to the language, which is opposite to the existing ``while`` operator. 
    Usage example:

    ```Python
    num = 3
    until num == 0:
        print num
        num -= 1
    ```
    Output of the above code:
    ```
    3
    2
    1
    ```
    
3. ``inc.patch`` adds ``++`` (increment) and ``--`` (decrement) operators to the language:

    ```python
    test = 1
    test--
    print test
    test++
    print test
    ```
    Output of the above code:
    ```
    0
    1
    ```
## Usage

``*.patch`` files can be applied to the ``cpython`` sources using git (checkout to the ``2.7`` tag is required):

```bash
git clone https://github.com/python/cpython.git 
cd cpython 
git checkout 2.7 
git apply patchname.patch
```

To build python:

```bash
./configure --with-pydebug --prefix=/tmp/python 
make
```

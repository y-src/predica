# predica

Predicates for use in higher-order functions.

## Tasks
- [x] Keep API as simple as possible. For instance, `P.*` to access all predicates.
- [x] `Predicates` or `P` should **only** evalaute as predicates or return predicates.
- [ ] Extend `P.and_` and `P.or` to take containers of predicates.
- [ ] Improve usage example.

## Installation

```bash
pip install predica
```

## Usage

Let's illustrate usage with three examples, gradually improving the composability of the code:

Simple example -

```python
from predica import P  # you can import `Predicates` too

ages = [1, 10, 100, 1000, '1']

# before
ages_no_str = list(filter(lambda x: not isinstance(x, str), ages)) # [1, 10, 100, 1000]

# after
new_ages_no_str = list(filter(P.not_instanceof(str), ages)) # [1, 10, 100, 1000]
```

Suppose you have a pre-existing function `is_str` -

```python
def is_str(x: str) -> bool:
    return isinstance(x, str)

# before
ages_no_str = list(filter(lambda x: not is_str(x), ages))  # [1, 10, 100, 1000]

# after
new_ages_no_str = list(filter(P.negation(is_str), ages))  # [1, 10, 100, 1000]
```

IMO this works best with `returns` [library](https://github.com/dry-python/returns) or `result` [library](https://github.com/rustedpy/result), following is example with `Result` from `returns.result` -


```python
from returns.result import Result
from functools import partial

ages = Result.from_value([1, 10, 100, 1000, '1'])

ages.map(partial(filter, P.not_instanceof(str))).map(list)  # Success([1, 10, 100, 1000])
# or
ages.map(partial(filter, P.negation(is_str))).map(list)  # Success([1, 10, 100, 1000])
```

Much better 😮

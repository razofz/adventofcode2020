---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.6.0
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

# Day 3

https://adventofcode.com/2020/day/3

---


The structure of the input lends itself well to being represented as a [sparse matrix](https://en.wikipedia.org/wiki/Sparse_matrix). Let's implement that with SciPy's [dok_matrix](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.dok_matrix.html#scipy.sparse.dok_matrix)!

## Part 1

```python
from scipy.sparse import dok_matrix
import numpy as np
```

## Test

```python
test = '''..##.......
#...#...#..
.#....#..#.
..#.#...#.#
.#...##..#.
..#.##.....
.#.#.#....#
.#........#
#.##...#...
#...##....#
.#..#...#.#
'''
```

```python
dok_matrix([[1 if y == '#' else 0 for y in x] for x in given.splitlines()])
```

```python
def massage_input(i):
    return dok_matrix([[1 if y == '#' else 0 for y in x] for x in i])
```

```python
def part1(p):
    width = len(p[0])
    height = len(p)
    p = massage_input(p)
    pointer = (0, 0)
    trees = 0
    while pointer[0] < height:
        trees += p.get(pointer, default = 0)
        pointer = (pointer[0]+1, (pointer[1] + 3) % width)
    return trees
```

```python
assert part1(test.splitlines()) == 7
```

### Run

```python
with open('day3-input.txt', 'r') as f:
    given = [x.strip() for x in f.readlines()]
```

```python
part1(given)
```

---

## Part 2


## Test

```python
def part2(p, x_step, y_step):
#     kolla = p
    width = len(p[0])
    height = len(p)
    p = massage_input(p)
    pointer = (0, 0)
    trees = 0
    while pointer[0] < height:
        trees += p.get(pointer, default = 0)
#         if p.get(pointer, default = 0) != 1 if kolla[pointer[0]][pointer[1]] == "#" else 0:
#             print(pointer, kolla[pointer[0]][pointer[1]])
        pointer = (pointer[0]+y_step, (pointer[1] + x_step) % width)
    return trees
```

```python
steps = [(1, 1), (3, 1), (5, 1), (7, 1), (1, 2)]
corrects = [2, 7, 3, 4, 2]
product = 1
for i in range(len(steps)):
    result = part2(test.splitlines(), steps[i][0], steps[i][1])
    assert result == corrects[i]
    product *= result
assert product == 336
```

```python
np.prod([part2(test.splitlines(), x[0], x[1]) for x in steps])
```

### Run

```python
np.prod([part2(given, x[0], x[1]) for x in steps])
```

```python
[part2(given, x[0], x[1]) for x in steps]
```

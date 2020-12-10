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

# Day 9

https://adventofcode.com/2020/day/9

---


## Part 1

```python
from IPython.core.debugger import set_trace
from itertools import combinations
import random
```

```python
def massage_input(i):
    i = [int(x) for x in i.splitlines()]
    return i
```

```python
def find_sum_terms(prev_nbrs, target_sum):
    for combo in combinations(prev_nbrs, 2):
        if combo[0] != combo[1]:
            if combo[0] + combo[1] == target_sum:
                return (combo[0], combo[1])
    return False
```

```python
def part1(p, length_preamble):
    p = massage_input(p)
    pointer = length_preamble
    while pointer < len(p):
#         print(pointer, p[pointer], pointer-length_preamble)#, p[nbrs[pointer]])
        nbrs_to_search = p[pointer-length_preamble:pointer]
        assert len(nbrs_to_search) == length_preamble
#         print('> nbrs_to_search:', nbrs_to_search)
        found_nbrs = find_sum_terms(nbrs_to_search, target_sum=p[pointer])
#         print('> found_nbrs', found_nbrs)
#             set_trace()
#         if found_nbrs:
#             p[nbrs[pointer]] = False
        if not found_nbrs:
            return p[pointer], pointer, nbrs_to_search
#         else:
#             pass
        pointer += 1
    return ...
```

Understanding the problem:

```python
for x in list(range(1,26)):
    print(x, end = ', ')
```

```python
l = list(range(1,20)) + list(range(21,26))
random.shuffle(l)
l
for x in [20] + l:
    print(x, end = ', ')
```

```python
i = 1
l = list(range(1,20)) + list(range(21,26))
random.shuffle(l)
for x in [20] + l:
    print(f'{i}: {x}')
    i += 1
```

```python
i = 1
l = list(range(1,20)) + list(range(21,26))
random.shuffle(l)
for x in [20] + l + [45]:
    print(f'{i}: {x}')
    i += 1
```

```python
find_sum_terms(l, 45)
```

```python
find_sum_terms(l + [45], 26)
```

```python
find_sum_terms(l + [45], 65)
```

```python
find_sum_terms(l + [45], 66)
```

```python
find_sum_terms(l + [45], 64)
```

Possible curveballs for part 2:

- product instead of sum
    - or other change of requirement
- change length of pre-amble
- take all numbers that does not meet requirement
- invalidate also the summed-together numbers


## Test

```python
tmp = [str(x) + '\n' for x in [20] + l]
test1 = ''
for x in tmp:
    test1 += x
del tmp
del x
length_preamble_test1 = 25
```

```python
print(test1)
```

```python
test2 = '''35
20
15
25
47
40
62
55
65
95
102
117
150
182
127
219
299
277
309
576
'''
length_preamble_test2 = 5
faulty_test2 = [127]
```

```python
# def massage_input(i):
#     i = i.splitlines()
#     out = {int(k): True for k in i}
#     return out
```

```python
massage_input(test1)
```

```python
massage_input(test2)
```

```python
assert find_sum_terms([x for x in range(3)], 0) == False
assert find_sum_terms([x for x in range(3)], 1) == (0, 1)
assert find_sum_terms([x for x in range(3)], 2) == (0, 2)
assert find_sum_terms([x for x in range(3)], 3) == (1, 2)
assert find_sum_terms([x for x in range(3)], 4) == False
```

```python
# %debug
find_sum_terms([47, 25, 15, 20, 35], 40)
```

```python
# def part1(p, length_preamble):
#     p = massage_input(p)
#     pointer = length_preamble
#     nbrs = list(p.keys())
#     while pointer < len(p):
# #         if p[nbrs[pointer]]:
# #             local_pointer = pointer - 1
# #             nbrs_to_search = []
# #             set_trace()
# #             while local_pointer >= 0 and len(nbrs_to_search) <= length_preamble:
# #                 if p[nbrs[local_pointer]]:
# #                     nbrs_to_search += [nbrs[local_pointer]]
# #                 local_pointer -= 1
# #             set_trace()
#         print(pointer, nbrs[pointer], pointer-length_preamble)#, p[nbrs[pointer]])
#         nbrs_to_search = nbrs[pointer-length_preamble:pointer]
#         assert len(nbrs_to_search) == length_preamble
#         print('> nbrs_to_search:', nbrs_to_search)
#         found_nbrs = find_sum_terms(nbrs_to_search, target_sum=nbrs[pointer])
#         print('> found_nbrs', found_nbrs)
# #             set_trace()
# #         if found_nbrs:
# #             p[nbrs[pointer]] = False
#         if not found_nbrs:
#             return nbrs[pointer]
# #         else:
# #             pass
#         pointer += 1
#     return ...
```

```python
part1(test2, length_preamble_test2)
# %debug
```

```python
# %debug
find_sum_terms([15,25, 47, 62, 55], 65)
```

```python
assert (out := part1(test2, length_preamble_test2))[0] == 127, out
```

### Run

```python
with open('day9-input.txt', 'r') as f:
    given = f.read()
```

```python
part1(given, 25)
```

---

## Part 2


## Test

```python
invalid = part1(given, 25)[0]
invalid
```

```python
def part2(p, invalid):
    p = massage_input(p)
    idx = p.index(invalid)
    
    for start in range(idx-1):
        for end in range(start+2, idx):
            nbrs = p[start:end]
            tmp = sum(p[start:end])
#             set_trace()
            if tmp > invalid:
                break
            if tmp == invalid:
                return p[start:end]
```

```python
out = part2(test2, 127)
out, sum([min(out), max(out)])
```

```python
out = part2(given, invalid)
out, sum([min(out), max(out)])
```

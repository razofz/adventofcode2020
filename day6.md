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

# Day 5

https://adventofcode.com/2020/day/5

---


## Part 1


## Test

```python
test = '''abc

a
b
c

ab
ac

a
a
a
a

b'''
```

```python
def massage_input(i):
#     i = [[answer if len(answer) == 1 else answer.split() for answer in group.split()] for group in i.split('\n\n')]
    out = []
    for group in i.split('\n\n'):
        answers = ''
        for answer in group.split():
            answers += answer
        out += [set(answers)]
#     i = [[[char for char in answer] if len(answer) > 1 else answer for answer in group.split()] for group in i.split('\n\n')]
#     i = [[answer for answer in group.split()] for group in i.split('\n\n')]
    return out
#     return [{field.split(':')[0]: field.split(':')[1] for field in passport} for passport in i]
```

```python
massage_input(test)
```

```python
def part1(p):
    return [len(group_answers) for group_answers in massage_input(p)]
```

```python
[len(group_answers) for group_answers in massage_input(test)]
```

```python
part1(test)
```

```python
corrects = [3, 3, 3, 1, 1]
correct_sum = 11
```

```python
p = part1(test)
for i in range(len(corrects)):
    assert p[i] == corrects[i]
assert sum(p) == correct_sum
```

### Run

```python
with open('day6-input.txt', 'r') as f:
    given = f.read()
```

```python
sum(part1(given))
```

---

## Part 2


## Test

```python
def massage_input_part2(i):
#     i = [[answer if len(answer) == 1 else answer.split() for answer in group.split()] for group in i.split('\n\n')]
    out = []
    for group in i.split('\n\n'):
        answers = ''
        for answer in group.split():
            answers += answer
        out += [set(answers)]
#     i = [[[char for char in answer] if len(answer) > 1 else answer for answer in group.split()] for group in i.split('\n\n')]
#     i = [[answer for answer in group.split()] for group in i.split('\n\n')]
    return out
#     return [{field.split(':')[0]: field.split(':')[1] for field in passport} for passport in i]
```

```python
def find_common_answers(sets):
#     print('> fca:', sets)
    if len(sets) == 0 and type(sets) == list:
        return []
    if len(sets) == 1 and type(sets) == list:
#         return [list(s) for s in list(sets[0])]
        return list(sets)[0]
    else:
        new_sets = []
        for l in itertools.permutations(range(len(sets))):
            perm_sets = [sets[x] for x in l]
#             print(perm_sets)
            for i in range(1, len(perm_sets)):
                new_sets += [set(perm_sets[0]).intersection(perm_sets[i])]
#         print(new_sets)
        out_sets = new_sets[0]
        for s in new_sets[1:]:
            out_sets = out_sets.intersection(s)
        return out_sets
```

```python
for group in test.split('\n\n')[:-1]:
    print(group)
    print(find_common_answers([answer for answer in group.split()]))
    print()
```

```python
def part2(i):
    i = [[answer for answer in group.split()] for group in i.split('\n\n')]
    out = []
    for group in i:
#         answers = group
        print('group', group)
        tmp = sorted(find_common_answers(group))
        common_answers = tmp
        print('common_answers', common_answers)
        print([len(common_answers)])
#         for j in range(len(answers[1:])):
#             common_answers.intersection(answers[j])
        out += [len(common_answers)]
    return out
```

```python
corrects_part2 = [3, 0, 1, 1, 1]
correct_sum_part2 = 6
result = part2(test)
for i in range(len(corrects_part2)):
    assert result[i] == corrects_part2[i], f'mine: {result[i]}, theirs: {corrects_part2[i]}'
assert sum(result) == correct_sum_part2
```

### Run

```python
check = [[answer for answer in group.split()] for group in given.split('\n\n')]
check[:10]
```

```python
i = 0
find_common_answers(sorted(check[i])), ["".join(sorted(x)) for x in check[i]]
```

```python
i = 1
# assert len(find_common_answers(check[i])) == 0
len(find_common_answers(check[i])), find_common_answers(check[i]), ["".join(sorted(x)) for x in check[i]]
```

```python
import itertools
for l in itertools.permutations(range(len(check[1]))):
    print(l)
    tmp = [check[1][x] for x in l]
    print(tmp)
    d = find_common_answers(tmp)
#     if len(d) > 0:
    print(d)
    print()
```

```python
find_common_answers(check[1])
```

```python
find_common_answers(check[2])
```

```python
# print(find_common_answers(check[0]))
# print(sorted(find_common_answers(check[0])))
# print(sorted({'h', 'w', 'x', 'u', 'm'}))
assert sorted(find_common_answers(check[0])) == sorted({'h', 'w', 'x', 'u', 'm'})
assert find_common_answers(check[1]) == set(), f'{find_common_answers(check[1])}'
assert find_common_answers(check[2]) == {'a', 'f', 'v'}
```

```python
i = 2
sorted(find_common_answers(check[i])), ["".join(sorted(x)) for x in check[i]]
```

```python
i = 3
sorted(find_common_answers(check[i])), ["".join(sorted(x)) for x in check[i]]
```

```python
i = 4
sorted(find_common_answers(check[i])), ["".join(sorted(x)) for x in check[i]]
```

```python
sum((out := part2(given)))
```

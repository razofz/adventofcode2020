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

# Day 6

https://adventofcode.com/2020/day/6

---


## Part 1

```python
import itertools
```

## Test

```python
test = """abc

a
b
c

ab
ac

a
a
a
a

b"""
```

```python
def massage_input(i):
    out = []
    for group in i.split("\n\n"):
        answers = ""
        for answer in group.split():
            answers += answer
        out += [set(answers)]
    return out
```

```python
massage_input(test)
```

```python
def part1(p):
    return [len(group_answers) for group_answers in massage_input(p)]
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
with open("day6-input.txt", "r") as f:
    given = f.read()
```

```python
sum(part1(given))
```

---

## Part 2


## Test


Doing this in a very exhaustive way: taking all the permutations of the intersected sets of characters (yes-answers) in the group. This is to ensure that we get a correct output, without this exhaustive approach some edge cases muddled the result. E. g. the second groups' answers, `['k', 'k', 'tl', 'k']`, gave output `k`, which is obviously not in every person's answers.

*(There's probably (very probable) some faster and cleaner way to solve this. But in line with the theme that seems to arise during these exercises: done is better than perfect :)*

```python
def find_common_answers(sets):
    if len(sets) == 0 and type(sets) == list:
        return []
    if len(sets) == 1 and type(sets) == list:
        return list(sets)[0]
    else:
        new_sets = []
        for permutation in itertools.permutations(range(len(sets))):
            perm_sets = [sets[x] for x in permutation]
            for i in range(1, len(perm_sets)):
                new_sets += [set(perm_sets[0]).intersection(perm_sets[i])]
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
        out += [len(find_common_answers(group))]
    return out
```

```python
corrects_part2 = [3, 0, 1, 1, 1]
correct_sum_part2 = 6
result = part2(test)
for i in range(len(corrects_part2)):
    assert (
        result[i] == corrects_part2[i]
    ), f"mine: {result[i]}, theirs: {corrects_part2[i]}"
assert sum(result) == correct_sum_part2
```

### Run

```python
check = [[answer for answer in group.split()] for group in given.split('\n\n')]
check[:10]
```

```python
i = 0
len(sorted(find_common_answers(check[i]))), find_common_answers(check[i]), ["".join(sorted(x)) for x in check[i]]
```

```python
i = 1
len(sorted(find_common_answers(check[i]))), find_common_answers(check[i]), ["".join(sorted(x)) for x in check[i]]
```

```python
i = 2
len(sorted(find_common_answers(check[i]))), find_common_answers(check[i]), ["".join(sorted(x)) for x in check[i]]
```

```python
i = 3
len(sorted(find_common_answers(check[i]))), find_common_answers(check[i]), ["".join(sorted(x)) for x in check[i]]
```

```python
i = 4
len(sorted(find_common_answers(check[i]))), find_common_answers(check[i]), ["".join(sorted(x)) for x in check[i]]
```

```python
assert sorted(find_common_answers(check[0])) == sorted({"h", "w", "x", "u", "m"})
assert find_common_answers(check[1]) == set(), f"{find_common_answers(check[1])}"
assert find_common_answers(check[2]) == {"a", "f", "v"}
```

```python
sum((out := part2(given)))
```

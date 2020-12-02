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

# Day 2

https://adventofcode.com/2020/day/2


---

## Part 1


## Test

```python
given = '''1-3 a: abcde
1-3 b: cdefg
2-9 c: ccccccccc
'''
```

```python
def massage_input(i):
    i = [x.split(' ') for x in i]
    i = [[tuple([int(y) for y in x[0].split('-')]), x[1].replace(':', ''), x[2]] for x in i]
    return i
```

```python
def part1(p):
    occurrences = p[2].count(p[1])
    if occurrences >= p[0][0] and occurrences <= p[0][1]:
        return 1
    else:
        return 0
```

```python
massage_input(given.splitlines())
```

```python
sum([part1(x) for x in massage_input(given.splitlines())])
```

### Run

```python
with open('day2-input.txt', 'r') as f:
    given = [x.strip() for x in f.readlines()]
```

```python
!wc -l day2-input.txt
```

```python
len(massage_input(given))
```

```python
massage_input(given)[:5]
```

```python
sum([part1(x) for x in massage_input(given)])
```

---

## Part 2


## Test

```python
test = '''1-3 a: abcde
1-3 b: cdefg
2-9 c: ccccccccc
'''
```

```python
def part2(p):
    try:
        # print(p[1], p[2][p[0][0]-1], p[2][p[0][1]-1], ( p[2][p[0][0]-1] == p[1] ) ^ ( p[2][p[0][1]-1] == p[1] ))
        return ( p[2][p[0][0]-1] == p[1] ) ^ ( p[2][p[0][1]-1] == p[1] )
    except IndexError:
        return False
```

```python
sum([part2(x) for x in massage_input(test.splitlines())])
```

### Run

```python
with open('day2-input.txt', 'r') as f:
    given = [x.strip() for x in f.readlines()]
```

```python
for line in massage_input(given):
    assert line[0][0]-1 >= 0, f'{line}'
```

```python
for line in massage_input(given):
    if line[0][0]-1 < 0:
        print(line)
```

```python
sum([part2(x) for x in massage_input(given)])
```

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
valids = {
'BFFFBBFRRR': {'row': 70, 'column': 7, 'seatID': 567},
'FFFBBBFRRR': {'row': 14, 'column': 7, 'seatID': 119},
'BBFFBBFRLL': {'row': 102, 'column': 4, 'seatID': 820}
}
example = 'FBFBBFFRLR' 
valids
```

```python
n_rows = 127
n_columns = 7
row_multiplier = 8
```

```python
def calc_seatID(row, column):
    return row * row_multiplier + column
```

```python
max_seatID = calc_seatID(n_rows, n_columns)
```

```python
test = '''FBFBBFFRLR
BFFFBBFRRR
FFFBBBFRRR
BBFFBBFRLL
'''
```

```python
from math import ceil
```

```python
def choose_half(selection_space, which_half):
    if which_half == 'lower':
        selection_space = (selection_space[0], selection_space[1] - ceil((selection_space[1]-selection_space[0]) / 2))
    elif which_half == 'upper':
        selection_space = (selection_space[0] + ceil((selection_space[1]-selection_space[0]) / 2)), selection_space[1]
    return selection_space
```

Validate functionality:

- Start by considering the whole range, rows 0 through 127.
- F means to take the lower half, keeping rows 0 through 63.
- B means to take the upper half, keeping rows 32 through 63.
- F means to take the lower half, keeping rows 32 through 47.
- B means to take the upper half, keeping rows 40 through 47.
- B keeps rows 44 through 47.
- F keeps rows 44 through 45.
- The final F keeps the lower of the two, row 44

```python
steps = [
    (0, 127),
    (0, 63),
    (32, 63),
    (32, 47),
    (40, 47),
    (44, 47),
    (44, 45),
    (44, 44)
]
```

```python
choose_half((0, 127), 'upper')
```

```python
choose_half((0, 1), 'lower')
```

```python
for i in range(7):
    e = 'upper' if example[i] == 'B' else 'lower'
    assert (out := choose_half(steps[i], e)) == steps[i+1], f'mine: {out} correct: {steps[i+1]}' 
```

![https://blog.electroica.com/wp-content/uploads/2020/09/walrus-operator-python.jpg](https://blog.electroica.com/wp-content/uploads/2020/09/walrus-operator-python.jpg)


> For example, consider just the last 3 characters of FBFBBFFRLR:

>   -  Start by considering the whole range, columns 0 through 7.
>   -  R means to take the upper half, keeping columns 4 through 7.
>   -  L means to take the lower half, keeping columns 4 through 5.
>   -  The final R keeps the upper of the two, column 5.

> So, decoding FBFBBFFRLR reveals that it is the seat at row 44, column 5.

```python
col_steps = [
    (0, 7),
    (4, 7),
    (4, 5),
    (5, 5) # changed this, it still gives correct answer in my solution
]
```

```python
for i in range(7, 10):
    e = 'upper' if example[i] == 'R' else 'lower'
    assert (out := choose_half(col_steps[i-7], e)) == col_steps[i-7+1], f'mine: {out} correct: {col_steps[i-7+1]}' 
```

```python
def check_boarding_pass(bp):
    row, column, seatID = (0, n_rows), (0, n_columns), 0
    if len(bp) != 10:
        return False
    for row_spec in bp[:7]:
        if row_spec not in ['F', 'B']:
            return False
        else:
            row = choose_half(row, 'upper' if row_spec == 'B' else 'lower')
    for col_spec in bp[7:]:
        if col_spec not in ['R', 'L']:
            return False
        else:
            column = choose_half(column, 'upper' if col_spec == 'R' else 'lower')
    assert row[0] == row[1]
    assert column[0] == column[1]
    seatID = calc_seatID(row[0], column[0])
    return row, column, seatID
```

```python
[print(bp, check_boarding_pass(bp)) for bp in test.splitlines()]
```

```python
for bp in valids.keys():
    checked =  check_boarding_pass(bp)
    print(bp, ':', checked)
    assert checked[0][0] == valids[bp]['row'], f'{checked[0][0]} != {valids[bp]["row"]}'
    assert checked[1][0] == valids[bp]['column'], f'{checked[1][0]} != {valids[bp]["column"]}'
    assert checked[2] == valids[bp]['seatID'], f'{checked[2]} != {valids[bp]["seatID"]}'
```

```python
def part1(p):
    IDs = []
    for bp in p:
        _, _, ID = check_boarding_pass(bp)
        IDs += [ID]
    return IDs
```

### Run

```python
with open('day5-input.txt', 'r') as f:
    given = [x.strip() for x in f.readlines()]
```

```python
given[:5]
```

Fulhack:

```python
given.sort()
```

```python
given[:5]
```

```python
for x in given[:5]:
    print(check_boarding_pass(x))
```

So answer seems to be 866.

```python
check_boarding_pass(given[0])
```

More exhaustive way:

```python
max(part1(given))
```

---

## Part 2


## Test

```python
def part2(p, x_step, y_step):
    return ...
```

### Run

```python
out = part1(given)
```

```python
out[:10]
```

```python
out.sort()
```

```python
out[:10]
```

```python
set(range(min(out), max(out)+1)).difference(set(out))
```

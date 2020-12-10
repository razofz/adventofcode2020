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

# Day 8

https://adventofcode.com/2020/day/8

---


## Part 1


So, we're basically dealing with an assembly language. A pointer, and a value counter (`acc`).


## Test

```python
test = '''nop +0
acc +1
jmp +4
acc +3
jmp -3
acc -99
acc +1
jmp -4
acc +6
'''
```

```python
test = test.splitlines()
```

```python
test
```

What instruction set do we have?

```python
pointer = 0
visited_instructions = set()
acc = 0

while pointer not in visited_instructions or pointer >= len(test):
    visited_instructions.add(pointer)
    instruction, value = test[pointer].split(" ")
    value = int(value)
    print("instruction:", instruction, ", value:", value)
    if instruction == "jmp":
        pointer += value
    else:
        if instruction == "acc":
            acc += value
        pointer += 1
acc
```

```python
with open('day8-input.txt', 'r') as f:
    given = f.read().splitlines()
```

```python
given[:10]
```

```python
pointer = 0
visited_instructions = set()
acc = 0

while pointer not in visited_instructions or pointer >= len(given):
    visited_instructions.add(pointer)
    instruction, value = given[pointer].split(" ")
    value = int(value)
#     print("instruction:", instruction, ", value:", value)
    if instruction == "jmp":
        pointer += value
    else:
        if instruction == "acc":
            acc += value
        pointer += 1
acc
```

```python
changees = [x.split(' ')[0] in ['jmp', 'nop'] for x in given]
sum(changees)
```

```python
len(given)
```

### Run

```python
ins_indices = []
for i in range(len(changees)):
    if changees[i]:
        ins_indices += [i]
```

```python
if 'visited_instructions' in locals():
    print(1)
```

```python
102 in ins_indices
```

```python
j = 0
for ins_index in ins_indices:
    existing_variables = locals()
    if 'visited_instructions' in existing_variables:
        del visited_instructions
    if 'pointer' in existing_variables:
        del pointer
    if 'acc' in existing_variables:
        del acc
    print(f'> ins_index: {ins_index}')
    pointer = 0
    visited_instructions = set()
    acc = 0
    ins_to_change = given[ins_index].split(' ')
    print(f'> ins_to_change: {ins_to_change}')
#     given_copy = [x for x in given]
    given_copy = given.copy()
    given_copy[ins_index] = f"{'jmp' if ins_to_change[0] == 'nop' else 'nop'} {ins_to_change[1]}"
    print(f'> given_copy[ins_index]: {given_copy[ins_index]}')
    print(f'> visited_instructions: {visited_instructions}')
    answer = False
    run = True
    i = 0
    while run == True or i < len(given):
#         print('run =', run)
        #     print(test[pointer])
        visited_instructions.add(pointer)
        instruction, value = given_copy[pointer].split(" ")
        value = int(value)
#         if pointer == ins_index:
#             print('\t!!> This is whats changed:')
#         print("\t> pointer:", pointer, "\tinstruction:", instruction, "\tvalue:", value, "\tacc:", acc)
        if instruction == "jmp":
            pointer += value
        else:
            if instruction == "acc":
                acc += value
            pointer += 1
        if pointer == len(given_copy):
            print('> ANSWER(?), acc:', acc)
            answer = acc
            run = False
            break
        if pointer in visited_instructions:
            print(f'> ERROR, pointer in visited_instructions: {pointer}')
            run = False
            break
        if pointer > len(given_copy) or pointer < 0:
            print(f'> ERROR, pointer outside index: {pointer}')
            run = False
            break
        i += 1
        if i > len(given):
            print("\t> pointer:", pointer, "\tinstruction:", instruction, "\tvalue:", value, "\tacc:", acc)
            raise Exception('inner loop continuing longer than length of given')
#     if pointer == len(given_copy):
    if answer:
        print('> ANSWER(?), acc:', acc)
        break
    print('------')
    j += 1
    if j > len(given):
        print('>ins_index:', ins_index)
        raise Exception('continuing longer than length of given')
acc
```

```python
raise Exception('continuing longer than length of given')
```

```python
ins_indices[:5]
```

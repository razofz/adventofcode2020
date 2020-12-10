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

# Day 7

https://adventofcode.com/2020/day/7

---


## Part 1

```python
import re
import random
```

## Test

```python
test = '''light red bags contain 1 bright white bag, 2 muted yellow bags.
dark orange bags contain 3 bright white bags, 4 muted yellow bags.
bright white bags contain 1 shiny gold bag.
muted yellow bags contain 2 shiny gold bags, 9 faded blue bags.
shiny gold bags contain 1 dark olive bag, 2 vibrant plum bags.
dark olive bags contain 3 faded blue bags, 4 dotted black bags.
vibrant plum bags contain 5 faded blue bags, 6 dotted black bags.
faded blue bags contain no other bags.
dotted black bags contain no other bags.
'''
```

```python
def massage_input(i):
    i = [x for x in i.splitlines()]
    i = [x.split(' bags contain ') for x in i]
    for j in range(len(i)):
        if i[j][1].startswith('n'):
            i[j][1] = []
        else:
            i[j][1] = [re.sub(r" bags?|\.", "", b) for b in i[j][1].split(', ')]
    return i
```

```python
with open('day7-input.txt', 'r') as f:
    given = f.read()
```

```python
test_colours = set()
for i in range(len((g := massage_input(test)))):
    assert len(g[i][0]) != 1, f'{g[i][0]}'
    if len(g[i][0]) == 1:
        print(g[i][0])
        print(g[i])
    colours.add(g[i][0])
colours
```

```python
global_colours = set()
for i in range(len((g := massage_input(given)))):
    assert len(g[i][0]) != 1, f'{g[i][0]}'
    if len(g[i][0]) == 1:
        print(g[i][0])
        print(g[i])
    colours.add(g[i][0])
random.sample(colours, 15)
```

```python
x = "1 striped silver bag, 3 dim brown bags, 2 bright cyan bags, 3 striped lime bags."
[re.sub(r" bags?|\.", "", b) for b in x.split(", ")]
```

```python
massage_input(test)
```

```python
t = massage_input(test)
check_set = set()
for l in t:
    check_set.add(type(l[0]))
assert len(check_set) == 1, f"{check_set}"
assert list(check_set)[0] == str, f"{check_set}"
```

```python
def build_bag_rules(bag_list, colours=global_colours):
    rules = {key: set() for key in colours}
    for bag in bag_list:
        for contained_bag in bag[1]:
            rules[bag[0]].add(contained_bag)
    return rules
```

```python
def part1(p):
    p = massage_input(p)
    for i in range(len(p)):
        p[i][1] = [re.sub(r"\d+ ", "", b) for b in p[i][1]]
    return p
```

```python
global_rules = build_bag_rules(part1(test))
```

```python
def find_that_which_shimmers(containing_bag, what_shimmers, rules=global_rules):
    #     print(containing_bag, ',', what_shimmers)
    inside_bag_set = rules[containing_bag]
    #     print('inside_bag_set:', inside_bag_set)
    if len(inside_bag_set) == 0 or containing_bag == what_shimmers:
        return False
    if what_shimmers in set(inside_bag_set):
        return True
    else:
        return True in [
            find_that_which_shimmers(inside_bag, what_shimmers, rules = rules)
            for inside_bag in inside_bag_set
        ]
```

```python
random.sample(part1(given), 15)
```

```python
part1(test)
```

```python
build_bag_rules(part1(given))
```

```python
g = part1(test)
test_colours = set()
for i in range(len(g)):
    assert len(g[i][0]) != 1, f'{g[i][0]}'
    test_colours.add(g[i][0])

test_rules = build_bag_rules(g, colours=test_colours)
assert sum([find_that_which_shimmers(colour,
                                     'shiny gold',
                                     rules=test_rules)
            for colour in test_colours]
           ) == 4
```

```python
test_rules
```

```python
# assert can_contain_bag(containing_bag, inside_bag)
assert find_that_which_shimmers("muted yellow", "shiny gold", rules=test_rules)
assert find_that_which_shimmers("dark orange", "shiny gold", rules=test_rules)
assert find_that_which_shimmers("light red", "shiny gold", rules=test_rules)
assert find_that_which_shimmers("bright white", "shiny gold", rules=test_rules)
for c in ["dark olive", "vibrant plum", "dotted black", "faded blue", "shiny gold"]:
    assert not find_that_which_shimmers(c, "shiny gold", rules=test_rules), c
```

### Run

```python
random.sample(massage_input(given), 20)
```

```python
# %%timeit
out = part1(given)
global_rules = build_bag_rules(out)
out
sum([find_that_which_shimmers(colour, 'shiny gold') for colour in colours])
```

---

## Part 2


## Test

```python
test2 = '''shiny gold bags contain 2 dark red bags.
dark red bags contain 2 dark orange bags.
dark orange bags contain 2 dark yellow bags.
dark yellow bags contain 2 dark green bags.
dark green bags contain 2 dark blue bags.
dark blue bags contain 2 dark violet bags.
dark violet bags contain no other bags.
'''
```

```python
def part2(p):
    p = massage_input(p)
    for i in range(len(p)):
        if len(p[i][1]) != 0:
            p[i][1] = [[x[0], x[2:]] for x in p[i][1]]
    return p
```

```python
part2(test2)
```

```python
part2(test)
```

```python
# %debug
def build_bag_rules_part2(bag_list, colours=global_colours):
    rules = {key: set() for key in colours}
    for bag in bag_list:
        #print(bag)
        for contained_bag in bag[1]:
            #print(contained_bag)
            rules[bag[0]].add(tuple(contained_bag))
    return rules
```

```python
test_rules2 = build_bag_rules_part2(part2(test), test_colours)
test_rules2
```

```python
part2(test)
```

```python
part2(test2)
```

```python
g = part2(test2)
test_colours2 = set()
for i in range(len(g)):
    assert len(g[i][0]) != 1, f'{g[i][0]}'
    test_colours2.add(g[i][0])

test_rules2 = build_bag_rules_part2(g, colours=test_colours2)
```

```python
test2_rules2 = build_bag_rules_part2(part2(test2), test_colours2)
test2_rules2
```

```python
def nbr_of_bags(containing_bag, inside_bags, rules=''):#global_rules2):
    print("containing_bag:", containing_bag, ", inside_bags:", inside_bags)
    inside_bag_set = rules[containing_bag]
    print("inside_bag_set:", inside_bag_set)
#     print(for inside_bag in inside_bags
#     if len(inside_bag_set) == 1:
#         return int(list(inside_bag_set)[0][0])
    if len(inside_bag_set) == 0:
        print('> end condition: ', containing_bag, inside_bag_set)
        return [1]
    return sum(
        [
            
                [
                    sum(x[0] * nbr_of_bags(inside_bag[1], x[1], rules=rules))
                    for x in rules[inside_bag[1]]
                ]
            
            for inside_bag in inside_bag_set
        ]
    )
```

```python
def nbr_of_bags(containing_bag, rules=""):  # global_rules2):
    inside_bags = list(rules[containing_bag])
#     print("containing_bag:", containing_bag, "; inside_bags:", inside_bags)
    #     inside_bag_set = rules[containing_bag]
    #     print("inside_bag_set:", inside_bag_set)
    #     print(for inside_bag in inside_bags
    #     if len(inside_bag_set) == 1:
    #         return int(list(inside_bag_set)[0][0])
    #     if len(inside_bags) == 0:
    #         print("> end condition: ", containing_bag, inside_bags)
    #         return 1
    return sum(
        [
            int(inside_bag[0])
            + int(inside_bag[0]) * nbr_of_bags(inside_bag[1], rules=rules)
            if len(list(rules[inside_bag[1]])) > 0
            else int(inside_bag[0])
            for inside_bag in inside_bags
        ]
    )
```

```python
print(list(test_rules2['shiny gold']))
```

```python
test_rules2['dotted black']
```

```python
# %debug
inp = 'shiny gold'
print(inp)
print(test_rules2[inp])
[x[1] for x in test_rules2[inp]]
nbr_of_bags(inp, rules=test_rules2)
```

```python
test_rules2.items()
```

```python
test_rules2['vibrant plum']
```

```python
[x[1] for x in test_rules2['vibrant plum']]
```

```python
check_test1 = ['dark olive', 'vibrant plum', 'dotted black', 'faded blue'] #, 'dark orange']
corrects_test1 = [7, 11, 0, 0]

# to_check_test1 = nbr_of_bags(part2(), test_rules2)
for i in range(len(corrects_test1)):
    assert (out := nbr_of_bags(check_test1[i], rules=test_rules2)) == corrects_test1[i], f'{out} {corrects_test1[i]}'
assert (out := nbr_of_bags('shiny gold', rules=test_rules2)) == 32, f'{out} != 32'
```

```python
assert (out := nbr_of_bags('shiny gold', rules=test2_rules2)) == 126, f'{out} != 126'
```

### Run

```python
global_rules2 = build_bag_rules_part2(part2(given), colours)
```

```python
global_rules2['shiny gold']
```

```python
nbr_of_bags('shiny gold', rules=global_rules2)
```

```python
%quickref
```

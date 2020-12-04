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

# Day 4

https://adventofcode.com/2020/day/4

---

```python
import re
```

## Part 1


## Test


The requirements for a valid passport:

```python
fields = {
    'byr': {'what': '(Birth Year)', 'length': 4, 'min': 1920, 'max': 2002},
    'iyr': {'what': '(Issue Year)', 'length': 4, 'min': 2010, 'max': 2020},
    'eyr': {'what': '(Expiration Year)', 'length': 4, 'min': 2020, 'max': 2030},
    'hgt': {'what': '(Height)', 'suffix': ['cm', 'in'], 'min': [150, 59], 'max': [193, 76]},
    'hcl': {'what': '(Hair Color)', 'prefix': '#', 'length': 6, 'class': '[0-9a-f]'},
    'ecl': {'what': '(Eye Color)', 'valid_cols': ['amb', 'blu', 'brn', 'gry', 'grn', 'hzl', 'oth']},
    'pid': {'what': '(Passport ID)', 'length': 9, 'class': '[0-9]'},
    'cid': {'what': '(Country ID)'}
}
"fields", fields
```

```python
test = '''ecl:gry pid:860033327 eyr:2020 hcl:#fffffd
byr:1937 iyr:2017 cid:147 hgt:183cm

iyr:2013 ecl:amb cid:350 eyr:2023 pid:028048884
hcl:#cfa07d byr:1929

hcl:#ae17e1 iyr:2013
eyr:2024
ecl:brn pid:760753108 byr:1931
hgt:179cm

hcl:#cfa07d eyr:2025 pid:166559648
iyr:2011 ecl:brn hgt:59in
'''
```

```python
def massage_input(i):
    i = [line.split() for line in i.split('\n\n')]
    return [{field.split(':')[0]: field.split(':')[1] for field in passport} for passport in i]
```

```python
massage_input(test)
```

```python
def part1(p):
    p = massage_input(p)
    print(fields.keys())
    valids = 0
#     for passport in p:
# #         print(passport.keys())
#         d = set(fields.keys()).difference(set(passport.keys())) 
# #         print(len(passport.keys()))
#         print(d)
#         valids += 1 if len(d) < 1 or (len(d) == 1 and d == {'cid'}) else 0
#         print(valids)
    print([1 if len((d := set(fields.keys()).difference(set(passport.keys())))) < 1 or (len(d) == 1 and d == {'cid'}) else 0 for passport in massage_input(p)])
    return ...
#     return valids
```

```python
def part1(p):
    return [1 if len((d := set(fields.keys()).difference(set(passport.keys())))) < 1 or (len(d) == 1 and d == {'cid'}) else 0 for passport in massage_input(p)]
```

```python
print('sum of', p := part1(test), 'is', sum(p))
```

```python
assert sum(part1(test)) == 2
```

### Run

```python
with open('day4-input.txt', 'r') as f:
    given = f.read()
```

```python
given.count('\n\n')
```

```python
len(massage_input(given))
```

```python
print('sum of', p := part1(given), 'is', sum(p))
```

---

## Part 2


## Test

```python
invalids = '''eyr:1972 cid:100
hcl:#18171d ecl:amb hgt:170 pid:186cm iyr:2018 byr:1926

iyr:2019
hcl:#602927 eyr:1967 hgt:170cm
ecl:grn pid:012533040 byr:1946

hcl:dab227 iyr:2012
ecl:brn hgt:182cm pid:021572410 eyr:2020 byr:1992 cid:277

hgt:59cm ecl:zzz
eyr:2038 hcl:74454a iyr:2023
pid:3556412378 byr:2007
'''
```

```python
valids = '''pid:087499704 hgt:74in ecl:grn iyr:2012 eyr:2030 byr:1980
hcl:#623a2f

eyr:2029 ecl:blu cid:129 byr:1989
iyr:2014 pid:896056539 hcl:#a97842 hgt:165cm

hcl:#888785
hgt:164cm byr:2001 iyr:2015 cid:88
pid:545766238 ecl:hzl
eyr:2022

iyr:2010 hgt:158cm hcl:#b6652a ecl:blu byr:1944 eyr:2021 pid:093154719
'''
```

```python
def massage_input_for_validation(i):
    i = [line.split() for line in i.split('\n\n')]
    return [{field.split(':')[0]: field.split(':')[1] for field in passport} for passport in i]
```

Continuing the manual data validation, since the "better" approaches took too much time today. See below for WIP on those approaches.

```python
def valid_input(key, value):
    if key not in fields.keys():
        return False
    if key in ['byr', 'iyr', 'eyr']:
        if len(value) != fields[key]['length'] or int(value) < fields[key]['min'] or int(value) > fields[key]['max']:
            return False
    if key == 'hgt':
        if len(value) < (min([len(x) for x in fields[key]['suffix']]) + min([len(x) for x in str(fields[key]['min'])])):
            return False
        if value[-2:] not in fields[key]['suffix']:
            return False
        if (hgt := int(value[:-2])) < fields[key]['min'][(pos := fields[key]['suffix'].index(value[-2:]))] or hgt > fields[key]['max'][pos]:
            return False
    if key == 'hcl':
        if value[0] != fields[key]['prefix']:
            return False
        if len(value[1:]) != fields[key]['length']:
            return False
        if not bool(re.compile(f'^{fields[key]["class"]}{"{"}{fields[key]["length"]}{"}"}$').match(value[1:])):
            return False
    if key == 'ecl':
        if value not in fields[key]['valid_cols']:
            return False
    if key == 'pid':
        if len(value) != fields[key]['length']:
            return False
        if not bool(re.compile(f'^{fields[key]["class"]}{"{"}{fields[key]["length"]}{"}"}$').match(value)):
            return False
    return True
```

```python
valid_input('hcl', '#12345f')
```

> Check singular values:

```python
assert valid_input('byr', '2002') == True
assert valid_input('byr', '2003') == False

assert valid_input('hgt', '60in') == True
assert valid_input('hgt', '190cm') == True
assert valid_input('hgt', '190in') == False
assert valid_input('hgt', '190') == False

assert valid_input('hcl', '#123abc') == True
assert valid_input('hcl', '#123abz') == False
assert valid_input('hcl', '123abc') == False

assert valid_input('ecl', 'brn') == True
assert valid_input('ecl', 'wat') == False

assert valid_input('pid', '000000001') == True
assert valid_input('pid', '0123456789') == False
```

> Check that valid passports are seen as valid:

```python
for passport in massage_input(valids):
    for field in passport.items():
        print(field[0], field[1])
        assert valid_input(field[0], field[1]) == True
```

> Check that invalid passports are seen as invalid:

```python
for passport in massage_input(invalids):
    c = [valid_input(v[0], v[1]) for v in passport.items()]
    assert False in c
```

```python
def part2(p):
    valid_passports = 0
    errors = {k: 0 for k in list(fields.keys()) + ['passport']}
    for passport in massage_input(p):
        valid = True
#         print(set(fields.keys()).difference(set(passport.keys())))
        if not (len((d := set(fields.keys()).difference(set(passport.keys())))) < 1 or (len(d) == 1 and d == {'cid'})):
            valid = False
            errors['passport'] += 1
        else:
            for key, value in passport.items():
                valid = valid_input(key, value)
                if not valid:
                    errors[key] += 1
                    break
#         print(valid)
        if valid:
            valid_passports += 1
    return valid_passports, errors
```

```python
m = massage_input(given)
corrects = {key: set() for key in fields.keys()}
errors = {key: set() for key in fields.keys()}

for passport in m:
    for key, value in passport.items():
        if key in corrects.keys():
            if valid_input(key, value):
                corrects[key].add(value)
            else:
                errors[key].add(value)
```

```python
# corrects
```

```python
# errors
```

```python
part2(test)
```

```python
part2(given)
```

```python
def part2(p):
    p = massage_input(p)
    print(fields.keys())
    valids = 0
#     for passport in p:
# #         print(passport.keys())
#         d = set(fields.keys()).difference(set(passport.keys())) 
# #         print(len(passport.keys()))
#         print(d)
#         valids += 1 if len(d) < 1 or (len(d) == 1 and d == {'cid'}) else 0
#         print(valids)
    print([1 if len((d := set(fields.keys()).difference(set(passport.keys())))) < 1 or (len(d) == 1 and d == {'cid'}) else 0 for passport in massage_input(p)])
#     return valids
```

### Run

```python
np.prod([part2(given, x[0], x[1]) for x in steps])
```

```python
[part2(given, x[0], x[1]) for x in steps]
```

---

## Validation approaches

Realised for part 2 that it would be nice to do a more formal data validation approach, and also put some modules for that in my toolbox. However, after giving that a try I realised it would take too much time for today, as I would also have to change the reading-in of the data. But, it was good to dip my feet in it, and there's a probability that I will get opportunity to use this in future problems this December.


### Schema

```python
from schema import Schema, And, Use, Optional, SchemaError
```

```python
schema = Schema({
    'byr': And(lambda n: len(n) == 4, lambda n: 1920 < 2002)},
    ignore_extra_keys=True)
#     'iyr': {'what': '(Issue Year)', 'length': 4, 'min': 2010, 'max': 2020},
#     'eyr': {'what': '(Expiration Year)', 'length': 4, 'min': 2020, 'max': 2030},
#     'hgt': {'what': '(Height)', 'suffix': ['cm', 'in'], 'min': [150, 59], 'max': [193, 76]},
#     'hcl': {'what': '(Hair Color)', 'prefix': '#', 'length': 6, 'class': '[0-9a-f]'},
#     'ecl': {'what': '(Eye Color)', 'valid_cols': ['amb', 'blu', 'gry', 'grn', 'hzl', 'oth']},
#     'pid': {'what': '(Passport ID)', 'length': 9},
#     'cid': {'what': '(Country ID)'}
# })
schema
```

```python
schema.validate(massage_input(test)[0])
```

```python
schema.validate({'byr': '1900'})
```

### Valideer

[https://github.com/podio/valideer]() 

Seems nice. 

```python
import valideer as V
from valideer import adapts, AdaptTo
```

```python
@adapts(i={'byr': AdaptTo(int)})
def adapt_input(i):
    return i
```

```python
adapt_input(massage_input(test)[0])
```

```python
massage_input_valideer(test)
```

```python
v_schema = {
    '+byr': V.Range("number", min_value=1920, max_value=2002),
    '+iyr': V.Range("number", min_value=2010, max_value=2020),
    '+eyr': V.Range("number", min_value=2020, max_value=2030),
    '+hgt': _,
    '+hcl': _,
    '+ecl': _,
    '+pid': _,
    'cid': _,
}
```

```python
validator = V.parse(v_schema)
```

```python
validator.is_valid(massage_input(test)[0])
```

```python
adapt_input = V.parse({
    'byr': V.AdaptTo(int),
    'iyr': V.AdaptTo(int),
    'eyr': V.AdaptTo(int)
})
```

```python
adapt_input.is_valid(massage_input(test)[0])
```

```python

```

```python
schema.validate(V.parse(massage_input(test)[0]))
```

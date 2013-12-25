Facets
======

A community-curated list of one-liners in Ruby

### Files

### Strings

### Arrays

### Hashes

Create a hash `h` from two arrays `k` and `v` of equal length that represent `h`'s keys and values, respectively:
```ruby
h = Hash[k.zip(v)]
```
Decompose a hash `h` into two arrays `k` and `v` that respresent `h`'s keys and values, respectively:

1) The straightforward way:
```ruby
k, v = h.keys, h.values
```
2) Or more cryptically for impressing your nerdy friends:
```ruby
k, v = *h.to_a.transpose
```

### Regular Expressions

### Calculations

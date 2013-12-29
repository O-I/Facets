Facets
======

A community-curated list of one-liners (or several-liners if elegance demands) in Ruby

### Files

### Strings

### Arrays

##### Create an `n`x`n` multiplication table represented by a 2-dimensional array:
```ruby
[*1..n].product([*1..n]).map { |arr| arr.reduce(:*) }.each_slice(n).to_a
```
Or, if you're using ActiveSupport:
```ruby
[*1..n].product([*1..n]).map { |arr| arr.reduce(:*) }.in_groups(n)
```

And in terms of clarity, this one's about as good as it gets:
```ruby
Array.new(n) { |x| Array.new(n) { |y| (x+1)*(y+1) } }
```

### Hashes

##### Create a hash `h` from two arrays `k` and `v` of equal length that represent `h`'s keys and values, respectively:
```ruby
h = Hash[k.zip(v)]
```
##### Decompose a hash `h` into two arrays `k` and `v` that respresent `h`'s keys and values, respectively:

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

##### Compute `n!` for all nonnegative integers n (returns `nil` otherwise):

```ruby
(1..n).reduce(1, :*) if n.is_a?(Integer) && n > -1
```

### Miscellaneous

##### Given an `m`x`n` matrix, return an array that represents the clockwise spiral path from the top left to the center. For example, given

    matrix = [[1,2,3],
              [8,9,4],
              [7,6,5]]
              
`spiral(matrix)` should return `[1, 2, 3, 4, 5, 6, 7, 8, 9]`

```ruby
def spiral(matrix)
  matrix.empty? ? [] : matrix.shift + spiral(matrix.transpose.reverse)
end
```

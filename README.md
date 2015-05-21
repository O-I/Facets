Facets
======

A community-curated list of one-liners (or several-liners if elegance demands) in Ruby

### Files, Folders, and Paths

##### Convert the contents of `filename` to a string
```ruby
File.open(filename, 'rb') { |file| file.read }
```

##### Get the current user's home directory
```ruby
Dir.home
```

This only works if `ENV['HOME']` is set, though. RubyTapas #10 gives us an alternative using `Etc` to find the current user's login name and passing that to `Dir.home`:
```ruby
Dir.home(Etc.getlogin)
```

### Strings

##### Calculate the Hamming distance between two strings `str1` and `str2` (returns `nil` if `str1` and `str2` are of unequal length):
```ruby
str1.chars.zip(str2.chars).reduce(0) { |sum, (x, y)| sum + (x == y ? 0 : 1) } if str1.size == str2.size
```

### Arrays

##### Remove all instances of value `l` from an array `a` and return the result
```ruby
a - [l]
```

Or `a -= [l]` if you would like to reasign the result to `a`. This is the most elegant way I've seen of doing this. Doing something like `a.delete(l)` returns `l` rather than the updated value of `a` which disrupts method chaining. You could always do `a.tap { |x| x.delete(l) }`, but I think the above line is superior and just as chainable if enclosed in parentheses.

##### Convert an array `a` with even index whose elements are alternating key value pairs (e.g., `[k1, v1, k2, v2,..., kn, vn]` into the respective hash `h`
```ruby
h = Hash[*a]
```
This is likely not a good idea if `a` happens to be a large dataset as the splat operator will expand all contents of the array to the stack. In that case, you can use `h = Hash[a.each_slice(2).to_a]`.

In the event `a` is already a 2D array of the form `[[k1, v1], [k2, v2],..., [kn, vn]]`, you can simply do `h = Hash[a]`. And from Ruby 2.1 onwards, you can simply use `Array#to_h`: `h = a.to_h`.

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

##### Calculate the frequency distribution of an array `a`:

There are many ways to do this, but using `Enumerable#each_with_object` seems to be the most idiomatic:
```ruby
a.each_with_object(Hash.new(0)) { |element, frequency| frequency[element] += 1 }
```

`Enumerable#reduce` also works, but feels clumsier. Notice that the block parameters are reversed compared to the above and the need to explicitly return the accumulator on each pass:
```ruby
a.reduce(Hash.new(0)) { |frequency, element| frequency[element] += 1; frequency }
```

And yet another way using Ruby 2.2+ that seems truest to a functional style:
```ruby
a.group_by(&:itself).map { |k, v| [k, v.size] }.to_h
```

##### Get all but the first element of an array `a`:

You can use parallel assignment in conjunction with the splat operator if you're planning on using the head:
```ruby
head, *rest = a
```

But if you just want the rest, using `drop` is better:
```ruby
a.drop(1)
```

Both are preferable to `a[1..-1]` in that they return `[]` rather than `nil` if `a` is empty.

##### Find all duplicate values in an array `a` (Ruby 2.2+):
```ruby
a.group_by(&:itself).select { |_, v| v.size > 1 }.keys
```

You can replace `group_by(&:itself)` with `group_by { |n| n }` for lesser versions of Ruby.

##### Calculate rolling averages of an array `a` over interval length `n`:
```ruby
a.each_cons(n).map { |interval| interval.reduce(:+) / n.to_f }
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

##### Weighted random sampling without replacement of a hash `h`'s keys whose values are weighted probabilities that sum to 1:
```ruby
h.max_by { |_, weight| rand ** (1.0/weight) }.first
```

This is essentially the weighted analogue of `Array#sample`. With Ruby 2.2+, we also have the analogue of `Array#sample(n)`:
```ruby
h.max_by(n) { |_, weight| rand ** (1.0/weight) }.map(&:first)
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

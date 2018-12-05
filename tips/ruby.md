## Assigning the rest of an Array to a variable

```ruby
a = [1, 2, 3]
b, *rest = *a

b    # => 1
rest # => [2, 3]
a    # => [1, 2, 3]
```

## Array destructuring in block arguments

```ruby
array = [
  [:key1, :value1],
  [:key2, :value2],
  [:key3, :value3],
  [:key4, :value4]
]

array.each do |key, value|
  puts "#{key}: #{value}"
end
```

## Hash#default_proc as default value

```ruby
layers = Hash.new do |layers, layer_name|
  layers[layer_name] = Hash.new(&layers.default_proc)
end

layers[:layer_1][:layer_2][:layer_3] = 'a secret'

layers # => { layer_1: { layer_2: { layers_3: 'a secret' } } }

```

## HEREDOC and method chaining

As an HEREDOC is a multi-line string syntactic sugar, then it’s possible to chain methods on it.

In this example, we remove the trailing spaces and \n from an SQL query

```ruby
sql = <<-SQL.gsub("\n", ' ').squish
SELECT MIN(u.popularity)
FROM users u
WHERE
  u.created_at <= #{Time.now} AND u.created_at >= #{Time.now - 7.days}
SQL

puts sql
```

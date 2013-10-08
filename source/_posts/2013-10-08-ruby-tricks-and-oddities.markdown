---
layout: post
title: "Ruby Tricks and Oddities"
date: 2013-10-08 06:34
comments: true
categories: 
---
Just looked through Julio Santos' <a href="https://speakerdeck.com/jsantos/ruby-things">"Ruby Things"</a> on Speaker Deck and learned some quirkly little things about Ruby.

Creating hashes from arrays. 2 different outcomes:
```ruby
Hash[["one", 1], ["two", 2]]
=> {["one", 1]=>["two", 2]} 

a=["one", 1], ["two",2]
=> [["one", 1], ["two", 2]] 

Hash[a]
=> {"one"=>1, "two"=>2} 
```

The <a href="http://ruby-doc.org/core-2.0.0/Array.html#method-i-zip">zip method</a> "converts any arguments to arrays, then merges elements of self with corresponding elements from each argument."
For example:
```ruby
a = [2, 5, 8]
b = [3, 6, 9]
[1, 4, 7].zip(a, b)
=> [[1, 2, 3], [4, 5, 6], [7, 8, 9]] 
```
Santos demonstrates that you can use zip to create hashes:
```ruby
keys = [:one, :two, :three]
values = [1, 2, 3]
zip = keys.zip(values)
=> [[:one, 1], [:two, 2], [:three, 3]] 
Hash[zip]
=> {:one=>1, :two=>2, :three=>3}
```

And yesterday while wishing a `collect_with_index` method existed, my group discovered that you can combine `each_with_index` with `collect`!
```ruby
class Array
  def make_list
    self.each_with_index.collect do |german, index|
      "#{index+1}. #{german}"
    end
  end
end

array = ["eins", "zwei", "drei"]
array.make_list
 => ["1. eins", "2. zwei", "3. drei"]
```
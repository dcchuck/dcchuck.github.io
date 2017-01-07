---
layout: post
title:  "Exponential vs. Linear Time"
date:   2017-1-7 12:00:00 -0400
categories: algorithms, ruby
---

Visualizing exponential vs. linear growth is as simple as plotting `y = x` vs. `y = x^2`. Recognizing it in your code is another thing.

Let's say we are given an array. Our boss tells us we need to check that array for duplicates and give them back an array with only unique elements. "If I see one duplicate you're fired!"

Wow they're not messing around. To be safe we decide to start with an empty array and add each element from our starting array to it, but only after making sure it's not in there already. 

```ruby
def random_array size
  Array.new(size) { rand(1..size) }
end

def check_for_dupilicates_in_array check_array, final_array
  check_array.each do |element|
    final_array.push element if !final_array.include? element 
  end
  final_array
end
```

`random_array` gives us our starting product to modify and `check_for_dupilicates_in_array` describes our first frantic attempt at doing our job. To start off we'll be reviewing arrays of size 10,000.

```ruby
check_for_dupilicates_in_array random_array(10000), Array.new
```

Then it happens. Like the great razor blade war of whatever year they came out with those 6 blade razors, people wanted bigger arrays. We stopped reviewing arrays of 10,000 and started reviewing arrays of 100,000. Our work output suffers, significantly more than anticipated.

We reach out for help and a friend informs us of a feature we had all along in ruby called `benchmark` that would allow us to see how long it takes for us to do something.

```ruby
require "benchmark"

# our working code here

Benchmark.bm do |bm|
  bm.report("1E4: ") { check_for_dupilicated_in_array(random_array(1000), Array.new) }
  bm.report("1E5: ") { check_for_dupilicated_in_array(random_array(10000), Array.new) }
  bm.report("1E6: ") { check_for_dupilicated_in_array(random_array(100000), Array.new) }
end
```

```
       user     system      total        real
1E4:   0.010000   0.000000   0.010000 (  0.003280)
1E5:   0.250000   0.000000   0.250000 (  0.249644)
1E6:  24.320000   0.040000  24.360000 ( 24.413419)
```

The jump from 100,000 to 1,000,000 does a great job of showing the exponential growth in time that occurs as we look through an array and examine another nested array along the way.

Armed with the power to measure our performance, we seek improvements. `Array` * `Array` = problem, so we call our friend who knew about `benchmark`. `"Make a hash."` Ok. We make our new method and throw a benchmark in there right away.

```ruby
## other code
def create_hash_from_array check_array, new_hash
  check_array.each do |element|
    new_hash[element.to_s] = "1"
  end
  new_hash
end

Benchmark.bm do |bm|
  bm.report("1E4: ") { create_hash_from_array(random_array(1000), Hash.new) }
  bm.report("1E5: ") { create_hash_from_array(random_array(10000), Hash.new) }
  bm.report("1E6: ") { create_hash_from_array(random_array(100000), Hash.new) }
end
```

```
       user     system      total        real
1E4:   0.000000   0.000000   0.000000 (  0.001319)
1E5:   0.010000   0.000000   0.010000 (  0.010179)
1E6:   0.150000   0.000000   0.150000 (  0.156402)
```

This time we're just creating a hash with a key for each element of the array and setting the value to one. If we come across the same element twice, we will just be updating the `key:value` pair to one. `Once per element == Linear Time`. The time growth isn't exactly linear but it's clearly not exponential.

We just realized though, our boss needs an array. So we ask our friend `Benchmark` but this time start with our job description. They tell us about `.uniq`, which comes as part of `Array` in ruby.

```ruby
require "benchmark"

Benchmark.bm do |bm|
  bm.report("1E4: ") { random_array(1000).uniq! }
  bm.report("1E5: ") { random_array(10000).uniq! }
  bm.report("1E6: ") { random_array(100000).uniq! }
end
```

```
       user     system      total        real
1E4:   0.000000   0.000000   0.000000 (  0.000923)
1E5:   0.010000   0.000000   0.010000 (  0.009700)
1E6:   0.090000   0.000000   0.090000 (  0.091487)
```

Here as we measure the performance of calling `.uniq!` on arrays growing by a factor of 10 we are not disappointed. Our frantic methods are dominated by the years of improvements and fundamentals built into the `Ruby` core. Not only is calling `uniq` the most efficient, but the increase in execution time most closely resembles linear growth.

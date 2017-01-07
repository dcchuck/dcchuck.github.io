---
layout: post
title:  "Exponential vs. Linear Time"
date:   2016-08-14 12:00:00 -0400
categories: algorithms, ruby, javascript
---

Exponential vs. Linear Time

Visualizing exponential vs. linear growth is as simple as plotting `y = x` vs. `y = x^2`. Recognizing it in your code is another thing.

Let's say we are given an array. Our boss tells us we need to check that array for duplicates and give them back an array with only unique elements. "If I see one duplicate you're fired!"

Wow they're not messing around. To be safe we decide to get started with a brand new array. We're going to add each element, one at a time, to that new array but only if we don't see it in our brand new array first. 
```ruby
def random_array size
  Array.new(size) { rand(1..size) }
end

def check_for_dupilicated_in_array check_array, final_array
  check_array.each do |element|
    final_array.push element if !final_array.include? element 
  end
  final_array
end
```
Here we've got 2 ruby methods - `random_array` gives us our starting product to modify and `check_for_dupilicated_in_array` described our first frantic attempt at doing our job. To start off we'll be reviewing arrays of size 10,000. It's boring but it pays the bills.
```ruby
check_for_dupilicated_in_array random_array(10000), Array.new
```
Our first few weeks on the job are going well. We're pumping through thousands of arrays a day and our boss is happy. No duplicates to date.
Then it happens. Like the great razor blade war of whatever year they came out with those 6 blade razors, people wanted bigger arrays. We stopped reviewing arrays of 10,000 and started reviewing arrays of 100,000. Our work output suffered significantly more than anticipated.
Our friend informs us of this tool we had all along in ruby called `benchmark` that would allow us to see how long it takes for us to review one array. 
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
Suspicions confirmed. The jump from 100,000 to 1,000,000 do a great job of showing the exponential growth in time when we decide to look through an array and then look through another array. Our `y = x` is now `y = x * x = x^2`.
Armed with the power to measure our performance, we seek improvements. `Array` * `Array` = problem, so we call our friend who knew about `benchmark`. `"Make a hash."`. Ok. We make our new method and throw a benchmark in there right away.
```ruby
## other code
def create_hash_from_unique check_array, new_hash
  check_array.each do |element|
    new_hash[element.to_s] = "1"
  end
  new_hash
end

Benchmark.bm do |bm|
  bm.report("1E4: ") { create_hash_from_unique(random_array(1000), Hash.new) }
  bm.report("1E5: ") { create_hash_from_unique(random_array(10000), Hash.new) }
  bm.report("1E6: ") { create_hash_from_unique(random_array(100000), Hash.new) }
end
```
```
       user     system      total        real
1E4:   0.000000   0.000000   0.000000 (  0.001319)
1E5:   0.010000   0.000000   0.010000 (  0.010179)
1E6:   0.150000   0.000000   0.150000 (  0.156402)
```
Rather than looking at an array for each element of the array, this time we're just creating a hash with a key for each element of the array and setting the value to one. This way if we happen to come across the same element more than once, we're just once again setting the key value pair to one again. No matter the size of the array, we'll only have to visit each element once. Linear time. Of course, the time growth isn't quite linear but it's clearly not exponential.
Of course our boss needs an array! So we ask our friend `Benchmark` for some help again `"What language are you working in? Ruby? Array has uniq!"`. 
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
Here we see many of perks of ruby wrapped up into one short post. Ruby is a powerful language with a lot of features right out of the box. Along with those features, measuring our performance is only a require statement away.

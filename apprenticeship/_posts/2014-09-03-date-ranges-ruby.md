---
layout: post
title: "Date Ranges in Ruby"
category: post
---

Yesterday me and my pairing partner came across an interesting situation. The feature we were working on involved taking in some data, constructing a date range, a year for example, and then filtering that data through that range to see which ones were within that range.

A simple example to demonstrate this:

{% highlight ruby %}
dates_sample_one = { person_1: Date.today + 10, 
	                 person_2: Date.today, 
	                 person_3: Date.today + 366, 
	                 person_4: Date.today + 95, 
	                 person_5: Date.today + 400 }
	                         
def filter_dates_with_include(dates)
  dates.select do |person, date|
    (Date.today..Date.today + 365).include?(date)
  end
end
	
filter_dates_with_include(dates_sample_one)
#=> { person_1, person_2, ... }
{% endhighlight ruby %}
	
The above is fairly simple; 5 people with different dates, some that go beyond a year and other's which fall within the year. The method has a fixed range of one year, starting from the current date and checks if the dates coming through fall within that year. Something like the above will work fine for situations where the data set is fairly small and the calculation is fairly simple. Once you're dealing with a decent size of data, then you have a problem.

To show this, I'll use Ruby's `Benchmark` [module](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/benchmark/rdoc/Benchmark.html#method-c-bm) and increase the data pool:

```ruby
require 'benchmark'
	
dates_sample_one = { person_1: Date.today + 10, 
	                 person_2: Date.today, 
                     person_3: Date.today + 366,
                     person_4: Date.today + 95, 
	                 person_5: Date.today + 400 }
	
dates_sample_two = {}
	
(1..730).to_a.shuffle.take(40).each do |days|
  dates_sample_two["person_#{days}"] = Date.today + days
end
                         
def filter_dates_with_include(dates)
  dates.select do |person, date|
    (Date.today..Date.today + 365).include?(date)
  end
end
		
# Benchmarking both
# this will probably vary a little bit but this is the output I got
	
Benchmark.bm do |x| 
  x.report { filter_dates_with_include(dates_sample_one) }
end
#=>      	   user     system      total        real
#=> times:  0.000000   0.000000   0.000000 (  0.002043) # measured in seconds

	
Benchmark.bm do |x| 
  x.report { filter_dates_with_include(dates_sample_two) }
end
#=>   user     system      total        real
#=> 0.040000   0.000000   0.040000 (  0.040604)
```
	
Here the second batch is made out of 40 random dates. When running the benchmark the impact on the performance is clear; It's nearly 20 times slower and that's just 40 records.

### Include vs Cover

As it's obvious from the above, using `include?` can be quite costly. When we ran the code it was taking quite a while to load the page but our assumption at the time was that there could be an issue with the API we were hitting in order to get the data. After running the tests however, it became obvious that the problem was elsewhere.

The [explanation](http://www.ruby-doc.org/core-2.1.2/Range.html#method-i-include-3F) of the `include?` method in the Range module states: ***"Returns true if obj is an element of the range, false otherwise"***. What that translates to is that when you have a range and you ask whether an element is in it, it will check that element against each entry of the range. If it finds it then it will stop at the point where it found the element and won't proceed with the search. The problem with this though is that in situations where the element is not within the range, it will go through the whole range checking each element. Efficiency fail.

After getting quite frustrated with the slow tests, a fellow apprentice asked if we were using `cover?`. After checking the [documentation](http://www.ruby-doc.org/core-2.1.2/Range.html#method-i-cover-3F) we realised that this was definitely the choice we should of been using. What `cover?` will do is take the beginning and the end of a range and check them against the given date. The below is an example of that when the end of the range is also included in the range (see [exclude_end?](http://www.ruby-doc.org/core-2.1.2/Range.html#method-i-exclude_end-3F)):
	
```ruby
date_to_check = Date.today + 30
Date.today <= date_to_check && date_to_check <= Date.today + 365
#=> true	
```

This is definitely way more efficient than include but I will let the data do the talking:

```ruby
# Running the same benchmarks but with cover instead
	
def filter_dates_with_cover(dates)
  dates.select do |person, date|
    (Date.today..Date.today + 365).cover?(date)
  end
end
	
Benchmark.bm do |x| 
  x.report { filter_dates_with_cover(dates_sample_one) }
end
#=>      	   user     system      total        real
#=> times:  0.000000   0.000000   0.000000 (  0.000055)

	
Benchmark.bm do |x| 
  x.report { filter_dates_with_cover(dates_sample_two) }
end
#=>   user     system      total        real
#=> 0.040000   0.000000   0.040000 (  0.000181) # Over 200 times faster than include!
```
	
Knowing the above will definitely make me think twice in regards to the choices I make. It's certainly easy to fall in the trap of following what *seems* to be the right choice. Having things like the `Benchmark` module, help with measuring each decision. It's all nice and beautiful when talking about the options and writing them down but at the end of the day, numbers rule.

---
layout: post
title: A Better Start
category: blog
tag:
- teaching
- programming
- mentoring
---
Last night I attended Codebar where I got to help a lady learning Ruby. It was her first time witnessing Ruby and in general learning about the Object Oriented paradigm. My experience with her perhaps convinced me even further that Ruby, or more in general OO, may not be the best tools to use as first options teaching someone programming.

She mentioned how hard she found it trying to model things and so I tried explaining what objects are and how they communicate with each other using messages. We started with the following example:

```ruby
"Name".downcase
=> "name"
```

I explained how `"Name"` is a String object and how we're sending it the message `downcase` to perform some operation. So far so good. Where it got complicated was when a line like this came up:

```ruby
def add(x) do
  puts x + 1
end
```

Understandably, she was trying to see the pattern that I had explained above but she was confused. After explaining a bit about what the code does, I showed her another way of writing the body of the method to do the same thing:

```ruby
puts(x.+(1))
```

Now part of the problem is how Ruby allows omitting certain things like parentheses around method arguments. What is also not obvious and clear is how `+` is a method on an object and how the result of that gets another message internally from `puts` (`to_s`) to do something else. That's where the idea of objects with methods attached to them that operate on their data falls apart and causes confusion. It is not the first time I've seen this reaction from a beginner.

After discussing a bit about what she found confusing, I mentioned functional programming and how that may be another option to start with. I mentioned a couple of languages and we looked at the following snippet of Elixir code:

```ruby
{a, b, c} = {:hello, "world", 42}
IO.puts(c)
```

I asked her if she could deduce what the result would be of the last line. "It will print 42" she said. When I asked why, she said that it seemed that `a` mapped to `:hello`, `b` to `"world"` and `c` to `42`. Pattern matching explained without me saying anything. The last line I had to explain how `IO` was a module and not a class like you would have in Ruby. I described how a module acts simply as a box that contains certain tools (functions) that you can use to do certain things. In this case we were using the `puts` tool from the `IO` toolbox to print `c`.

This was a small example but the above experience and the experience I'm having with my student apprentice, who I also started with a functional language (Elixir), makes me believe that thinking in terms of functions with input and output instead of objects that have attached behaviour in the form of messages, may indeed be better.

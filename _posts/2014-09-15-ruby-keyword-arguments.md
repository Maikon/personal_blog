---
layout: post
title: "Ruby's Keyword Arguments"
category: blog
---
After few weeks working with Rails, this week I'm back working on my TicTacToe game in Ruby. I have quite few things to refactor but I'm really enjoying it since I'm applying certain [SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) principles as well as things I've learned from consuming resources on the [Hexagonal Architecture](http://alistair.cockburn.us/Hexagonal+architecture) as well as [Clean Architecture](http://blog.8thlight.com/uncle-bob/2011/11/22/Clean-Architecture.html). 

I will at some point write about those also on separate posts but for this one I'll write about a relatively new feature in Ruby which I started using today: ***Ruby Keyword Arguments***.

Prior to Ruby 2.0 if you wanted to have some more context with your method arguments you could pass a hash like so:

```ruby
def print_full_name(attrs)
  puts "My name is #{attrs[:first]} #{attrs[:last]}"
end
	
full_name = { first: 'Joe', last: 'Doe' }
print_full_name(full_name)
#=> "My name is Joe Doe"
```

With Ruby 2.0 though you can specify keyword arguments instead:

```ruby
def print_full_name(first: first, last: last)
  puts "My name is #{first} #{last}"
end
	
print_full_name(first: 'Joe', last: 'Doe')
#=> "My name is Joe Doe"
```

Now if I was to remove the braces from the hash that I passed in the first example you could say that the two approaches look identical. Well not quite. 

With keyword arguments you can specify default values:

```ruby
def print_full_name(first: 'Joe', last: last)
  puts "My name is #{first} #{last}"
end
	
print_full_name(last: 'Doe')
#=> "My name is Joe Doe"
```

Using a hash you could of course use `fetch` and provide a default value but that just adds clutter especially when doing so for multiple values.
Another nice thing about keyword arguments is what happens when you forget to provide an argument:

```ruby
def print_full_name(first: , last: last)
  puts "My name is #{first} #{last}"
end
	
print_full_name(last: 'Doe')
#=> ArgumentError: missing keyword: first
```

It lets you know which argument is missing so even when throwing an error, it's quite clear what the problem is. Its's worth noting that the arguments are order independent. You could switch the order of the arguments when calling the method and you will still get the same result. *Except today I discovered that might not always be the case*.

I was refactoring my computer class and after a small shuffle my `make_move` method looked like this:

```ruby
def make_move(board: board, move: minimax(board))
  board.mark_position(move, mark)
end
```

The `move` argument had a default value which was the result of calling the method `minimax` with the board as an argument. Now in my initial tests I had only added the `board` keyword so I was calling it with the following setup:

```ruby
board = TicTacToeCore::Board.new
computer = TicTacToeCore::Computer.new
computer.make_move(board: board)
```

I was expecting all of them to fail. They didn't though. I had no constructor and I wasn't storing that `Board` instance anywhere so naturally I was a bit surprised. After looking closely though the only thing that made sense to me was that when the call to `minimax` was made with `board` as an argument, it looked to its left and saw that a value named `board` was declared and it used it. There was only one way to test my theory; switch the order of the arguments: 

```ruby
def make_move(move: minimax(board), board: board)
```
Sure enough after doing the above every single test fell apart.

Now it's kind of nice that it did the lookup earlier but I don't think I'm a fan of this sort of 'magic'. It also creates what the late Jim Weirich would call [Connascence of Position](http://en.wikipedia.org/wiki/Connascence_(computer_programming)). His talk [here](https://www.youtube.com/watch?v=HQXVKHoUQxY) is very informative.

Since I've only started using keyword arguments recently, I can't say I have a solid opinion on them but so far I do like them as they provide context and are easy to use. We shall see what the future holds.

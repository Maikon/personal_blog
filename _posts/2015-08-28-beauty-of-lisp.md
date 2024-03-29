---
layout: post
title: The Beauty of Lisp
category: blog
tag:
- lisp
- clojure
- functional
---
Lisp has profoundly changed the way I look at languages. Even though I only have a small amount of experience with Lisp and its dialects, I find the simplicity of the language itself, stunning. I heard many people enthusiastically talking about this idea of simplicity within a Lisp. I never fully understood this idea and I'm sure I don't fully understand it yet, however I feel I have a pretty good idea now of what the meaning of that word is within the context of Lisp.

Unsurprisingly the main ingredient of this simplicity is its syntax or lack thereof. In it's simplest form it follows the rule:

```
(function argument1 argument2)
```

Function on the left, argument(s) on the right. This is, for me, where naturally this simplicity derives from. It only became more apparent today when I was looking at [Emojilisp](http://emojilisp.com/). If you observe the Fibonacci function on the bottom, understandably it will look scary and confusing. However, applying the rule from above, you can start seeing the pattern there:

```
(defun function_name (argument)
  (if (or (= argument 0) (= argument 1))
    1
    (+ (function_name (- argument 1)) (function_name (- argument 2)))))
```

Even with the Emojis cluttering the screen, the structure of Lisp elegantly and gloriously, shines through. Wonderful.

---
layout: post
title: "Roman Numerals in Java"
category: blog
tag:
- kata
- java
---

Many developers when trying to learn a new language, they use a kata they're familiar with. Since I started learning Java and having done Roman Numerals several times already in Ruby, I found it appropriate to use it in order to explore Java.

Below is a video of me starting the kata and stopping at the usual point once the algorithm becomes clear and the first two special cases, 4 and 9, are in place. Underneath the video you will find the full implementation.

<iframe width="640" height="480" src="//www.youtube.com/embed/HEh2NNuF-oE" frameborder="0" allowfullscreen></iframe>

<br/>
{% highlight java %}
class RomanNumeral {
  public static String convert(int number) {
    String roman = "";
    int[] arabic_numerals   = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    String[] roman_numerals = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    for (int i = 0; i < arabic_numerals.length; i++) {
      while (number >= arabic_numerals[i]) {
        roman += roman_numerals[i];
        number -= arabic_numerals[i];
      }
    }
    return roman;
  }
}

{% endhighlight %}

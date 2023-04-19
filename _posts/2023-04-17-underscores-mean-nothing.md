---
layout: post
title:  "Underscores Mean Nothing (to Python ints)"
date:   2023-04-17
categories: readability Python
---

This is a cool Python feature that is easy to miss because it...um...doesn't
do anything. You can include an underscore when declaring an int and the underscore
is ignored. Here's an example.

{% highlight python %}
>>> my_int = 100_000

>>> print(my_int)
100000
{% endhighlight %}

That's cool! You can add underscores to make integers easier to read. 

## Big numbers

Let's say that need to know how many Subway restaurants there were in the 
United States from 2015 to 2021[^1].

{% highlight python %}
us_subways = {
    2015: 27103,
    2016: 26744,
    2017: 25908,
    2018: 24798,
    2019: 23801,
    2020: 22005,
    2021: 21147,
}
{% endhighlight %}

You can add an underscore to make it easier to read.

{% highlight python %}
us_subways = {
    2015: 27_103,
    2016: 26_744,
    2017: 25_908,
    2018: 24_798,
    2019: 23_801,
    2020: 22_005,
    2021: 21_147,
}
{% endhighlight %}

APIs often use large integers as constants. When they do,
it can be difficult to see that 111111111 and 1111111111 are different. While those
two numbers appear similar to the human eye, they can have radically different meanings.

{% highlight python %}
RESERVED = 111111111
SELF_DESTRUCT = 1111111111
{% endhighlight %}

These constants are tough to check during code review. I know they are different because
I typed them and quadruple-checked them. Every time I read this code snippet,
they still look the same to me.

Let's add some underscores.

{% highlight python %}
RESERVED = 111_111_111
SELF_DESTRUCT = 1_111_111_111
{% endhighlight %}

Now it's easy to see the difference! Hooray!

Inspired by the Python convention, I use underscores when I write requirements
and stories. During design and planning, constants are repeatedly copied and rewritten.
Sometimes they are changed accidentally. Before you know it, your laptop is initiating a 
self-destruct sequence.

Adding underscores in the requirements phase makes these numbers easier to read and less
prone to typos or copy/paste errors.

## Weird units

The underscores don't have to go where commas go. You can put them anywhere. Consider a
thermostat control with a precision of 0.1 degrees Celsius. The interface accepts an integer.

Let's set the temperature to 20 degrees Celsius (68 degrees Farenheit).

{% highlight python %}
thermostat.set_temperature(200)
{% endhighlight %}

The code above looks like it was written by a maniac. Was I planning to watch
TV or bake potatoes? I wish my roommates would watch TV more quietly.
Maybe I wanted to make baked couch potatoes.

We can regain our sanity by adding an underscore where the decimal would go.

{% highlight python %}
thermostat.set_temperature(20_0)
{% endhighlight %}

Much better!

## Bit sequences masquerading as integers

You can use underscores with any integer format. Feel free to scatter them into your
binary too. This is helpful for numbers that represent bit arrays.

Our thermostat has an LED. We control the mode and brightness
with an integer that represents the following 8 bit sequence.

```
1 bit:
    0 = OFF
    1 = ON

2 bits: Mode
    00 = CONSTANT
    01 = FAST BLINK
    10 = SLOW BLINK
    11 = ATERNATES FAST/SLOW BLINK

5 bits: Brightness
    Number from 0 to 31
```

We want to configure ON/FAST BLINK/MID BRIGHTNESS. We could write

{% highlight python %}
configure_led(0b10101111)
{% endhighlight %}

Or we could add underscores to make it easier to see different parts of the command.

{% highlight python %}
configure_led(0b1_01_01111)
{% endhighlight %}

I find the second one easier to read. I also find it easier to type.

Some systems have more complex bit sequences than the the one above. Your mobile phone
receives scheduling information via Downlink Control Information 
(DCI)[^2]. Don't worry about what that means or how it works. Just check out this example
using DCI Format 2A for LTE.

{% highlight python %}
dci_2a = 0b0000100000000000000001011100000010000000
{% endhighlight %}

Here it is broken into its parts with underscores.

{% highlight python %}
dci_2a = 0b0_00010000000000000_00_010_1_11000_0_00_10000_0_00
{% endhighlight %}

Four of the chunks above have variable length. You can work out what the lengths are,
but I think it's nicer to use underscores so I only have to work the lengths out once.

## Start underscoring

I love that Python ints support underscores because they are quick and easy to use
when I need them. I can toss an underscore into a ticket description, source code,
test code, or the Python interactive console.

And just as importantly, I can leave them out when I don't want them. They're great
because they don't do anything. They add meaning to me and Python ignores them.

---

[^1]: Statista, [Number of Subway Restaurants in the United States from 2015 to 2021](https://www.statista.com/statistics/469341/number-of-subway-restaurants-us/), retrieved April 17, 2023.
[^2]: Here's are a couple of links to ShareTechnote that describe DCI: [4G/LTE - DCI](https://www.sharetechnote.com/html/DCI.html), [5G/NR - DCI](https://www.sharetechnote.com/html/5G/5G_DCI.html)


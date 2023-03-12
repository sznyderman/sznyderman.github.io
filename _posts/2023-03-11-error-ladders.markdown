---
layout: post
title:  "Error Ladders"
date:   2023-03-11
categories: errors python
---


You're walking through a beautiful park. There's a lazy gravel path to guide you, inviting lawns
to explore, and tall trees to shade you from the sun. As you admire the branches of an oak tree, you don't notice
the twelve foot hole in the path. You fall into the hole. You're bruised, sore, and stuck. You
have to find a way to climb out.

You are the victim of an error. You fell into a hole that either shouldn't be there or that you should have
known to walk around (wells are holes that you shouldn't be able to fall into).

When we build software, we turn an empty space into a park. Hopefully, users enjoy a nice walk or lie down 
in the shade. But sometimes they discover problems. Errors are holes that they fall into. 
A deep hole with steep walls is difficult to climb out of. But if we install a ladder, then they can climb out. 
That's the error ladder.

Errors happen. Some are avoidable; some are not. When we develop software, we should account for possible
error conditions and make them as painless as possible for users, support engineers, and integration teams. The error ladder is 
a way of thinking about when and how to raise errors. It started as a way to explain exceptions to junior engineers, 
although it applies to errors outside of engineering. It is not an exhaustive description of errors.
It helps you think about the purpose of exceptions so that you can write better ones.

There are four steps on the ladder:
* WHAT went wrong
* WHY it happened
* HOW to fix it
* FIX it automatically


## Package Delivery

We own a delivery company: Fast Package Service (FPS). Customers rely on FPS to get their packages to the correct
destination.

Marty wants to send a birthday present to his friend Charlie. He wraps the present and puts it in a cardboard
box. He writes Charlie's address on the box, attaches postage, and drops it off at FPS. One week
later, he calls Charlie on her birthday and learns that the package never arrived. Usually, delivery takes two
to three days. Maybe it is taking longer than normal and is still in transit. Marty calls Charlie a few weeks later.
Still no package. Will it ever arrive? Was it lost? Should Marty mail another package?

### WHAT went wrong

It would help if FPS told Marty that something went wrong. FPS could have returned the package
to Marty the day after he sent it with a note that said "not delivered". That's not great service, but it is
better than what happened. Marty would have known that something went wrong in a day instead
of a week. And he would have known the package wasn't lost or delayed. It wasn't delivered.

That makes solving the problem easier for Marty. Maybe it was a fluke and if he resends the package everything
will work. He could check that the postage was correct. He could try sending it with FedEx. He still has some
serious debugging to do, but at least he got quick feedback that something went wrong.

### WHY it happened

It would be better if the package had a note that explained why the package wasn't delivered. The returned
package could have included a more detailed note: "not delivered, invalid address". That simplifies the problem.
Marty has the wrong address or he wrote it on the package incorrectly. He can confirm the address with
Charlie, fix it on the package, and resend it.

He doesn't know what's wrong with the address, but at least he knows that the address is the underlying issue.

### HOW to fix it

Marty knows the address is wrong, but he doesn't know what is wrong with it. Did he provide a bad house number?
Maybe he provided a street that doesn't exist. He could have misspelled the city.

FPS could tell Marty what is wrong with the address and how to fix it:

> We are unable to deliver your package because of a problem with the address. The address has a four digit zipcode: 7825.
  Zipcodes should have five digits. Please update the zipcode and send your package again.

Okay! Now we're getting somewhere. Marty knows that the package wasn't delivered because he made a mistake
while writing down the zipcode. He doesn't need to confirm the address with Charlie. He just needs to
add the missing zipcode digit and resend the package.

### FIX it automatically

Frankly, FPS could have figured out the intended address without returning the package. The street and city is sufficient
to identify an address. FPS could look up the correct zipcode from the street and city and then deliver
the package accordingly. In fact, a real delivery service would do exactly that.

## Debugging

The error ladder mirrors my process for debugging. When presented with a problem,
I try to figure out what went wrong, why it happened, and how to fix it. If I think I will encounter the problem again,
then I try to automate the solution. I do this with software. I do it in the physical world too.

The error ladder leaves bread crumbs in advance so that some (or all) of the debugging is complete before the error
happens.

## Code

Let's revisit the FPS example in Python and see how the error ladder guides our code.

Here's what we want to do:

{% highlight pythone %}
package = Package()
fps = Fps()

fps.send_package(package)
{% endhighlight %}

Here's our implementation of Fps:

{% highlight pythone %}
class Fps:
    def __init__(self):
         self._driver = Driver()

    def send_package(self, package):
         self._driver.deliver(package)
{% endhighlight %}


### WHAT went wrong

Just like in our real world example, send_package(...) doesn't return a value. If the package never gets to its
destination, we would never know. As a starting point, let's ask the driver if the package was delivered and
raise an error if there is a problem. This way, the user knows what went wrong. [^1]

{% highlight pythone %}
class Fps:
    def send_package(self, package):
        self._driver.deliver(package)

        if not self._driver.was_it_delivered():
            raise RuntimeError("The package wasn't delivered.")
{% endhighlight %}

We could further improve on this by creating a custom error that indicates what happened instead of using a standard
Python error. It's easier to read a log where the error name tells you what is wrong. And it is more
flexible for client code to be able to catch specific error types without parsing the error message. [^2]

{% highlight pythone %}
class PackageNotDeliveredError(RuntimeError):
    pass

class Fps:
    def send_package(self, package):
        self._driver.deliver(package)

        if not self._driver.was_it_delivered():
            raise PackageNotDeliveredError()
{% endhighlight %}

### WHY it happened

We don't just want to tell the user that the package wasn't delivered. We want to explain why. Let's handle the
invalid address example. We could have known this problem before asking the driver to deliver the package.

{% highlight pythone %}
class InvalidAddressError(RuntimeError):
    pass

class PackageNotDeliveredError(RuntimeError):
    pass

class Fps:
    def send_package(self, package):
        if not is_valid_address(package.address):
            raise InvalidAddress(
                f"Unable to deliver the package because the address isn't "
                f"valid: {package.address}"
            )

        self._driver.deliver(package)

        if not self._driver.was_it_delivered():
            raise PackageNotDeliveredError()
{% endhighlight %}

We added our WHY error for invalid address. We also kept the PackageNotDeliveredError. As your software evolves, you will 
discover increasingly specific errors. As a result, you may keep a general WHAT error and continue adding
more helpful WHY errors as you discover them. Your error ladder is becoming more robust as time goes on.

### HOW to fix it

As we add more rungs to the ladder, the error information becomes more specific. There are multiple reasons why an address might be
invalid and how to fix it. Let's handle the zipcode error.

{% highlight pythone %}
def raise_if_invalid_address(address):
    zipcode_length = len(address.zipcode)
    if zipcode_length != 5:
        raise InvalidAddressError(
            f"Unable to deliver the package because the zipcode has "
            f"{zipcode_length} digits. It should be five digits."
        )

    ...handle other problems with address

class Fps:
    def send_package(self, package):
        raise_if_invalid_address(package.address)

        self._driver.deliver(self, package)

        if not self._driver.was_it_delivered():
            raise PackageNotDeliveredError()
{% endhighlight %}

Our error detection is becoming more complex, so we moved it into its own function: raise_if_invalid_address(..). That 
improves the code readability by clearly encapsulating the error handling.

There's also a logic to the error type and error message. The error type InvalidAddressError tells us what happened. The error message tells us
why it happened and how to fix it. The error message is human-readable English. It's not a stack trace or a glimpse into some deeply
technical debug info. It's a friendly explanation of what went wrong and what the user should do next.

### FIX it automatically

And finally, we automatically resolve the zipcode error.

{% highlight pythone %}
class Fps:
    def send_package(package):
        if not is_valid_address(package.address):
            package.address = repair_address(package.address)

        self._driver.deliver_package(package)

        if not self._driver.was_it_delivered():
            raise PackageNotDeliveredError()
{% endhighlight %}



## There will always be errors

The top rung of the error ladder, FIX it automatically, promises a world where users never experience errors because everything
is automatically resolved. Unfortunately, there will always be errors that we can't handle. What if the
package has no address? There's no way to figure out the intended destination with zero information. We could tell the user HOW to
fix it (provide an address), but we can't do it for them.

There are also times where an automatic "fix" is possible, but undesirable. Consider the control software for an infusion pump that
delivers intravenious medicine to a patient. A nurse or doctor enters a dose that is too high. It could be a simple typo. The pump's control software could
"fix" the error by providing the maximum dosage that the infusion pump supports. But it shouldn't. The user should be notified of the
error and asked to provide the correct dose. Any assumptions could be dangerous or even life threatening.

The infusion pump example highlights the importance of providing good errors. If the system ignores unsupported doses (like in the FPS example),
then a patient isn't receiving medication. From 2005 to 2009 there were 56,000 incidents with infusion pumps reported to the FDA, including numerous
injuries and deaths.[^3] The infusion pump example warrants more design consideration than the error ladder, but the ladder is
still an important part of the solution.

We won't always build a ladder with all four rungs, but we should build as tall of a ladder as possible.


## Be kind to yourself and others.

Software errors are real and range from inconvenience to death. It isn't practical to handle every possible error. You are unlikely to discover
every possible error in a system that you are working on. Nevertheless, you should actively consider what errors could occur and build a ladder for them. When
you discover a new problem in the field, don't just put in a quick-fix, build a new ladder.

Error ladders makes life easier for people who encounter errors in the future: users, support engineers, other developers, and even
you. Be kind to others and to your future self by including error ladders.




---
[^1]: We don't have to raise errors. We could return a tracking number that the customer uses to check their package's status.
      In this example, we're using exceptions. The principles of the error ladder still apply with other strategies.

[^2]: I have written a lot of code over the years that catches generic errors from third party libraries and then
      parses the error message to decide what action to take. It works, but it's messy. It
      also makes for poor documentation because you don't see the types of errors enumerated in the documentation.

[^3]: FDA, [Infusion Pump Improvement Initiative](https://www.fda.gov/medical-devices/infusion-pumps/white-paper-infusion-pump-improvement-initiative),
      April 2010, retrieved March 9 2023. 
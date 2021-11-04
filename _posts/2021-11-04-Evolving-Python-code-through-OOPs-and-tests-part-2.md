---
published: true
layout: post
title: Evolving Code through OOPs and tests - Part 2
---


I hope you have gone through the first part here.  We will continue to build on the same blocks and try to add more functionality to our seemingly simpe class.



> **Requirement**: You should be able to change the side for the square object. In doing so, perimeter as well as area attributes should get updated.

First we will add a test function wheree can change the side value of an object and perimeter,area attributes should update based on this new value

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-16-01-27.png)
*test function to check that updating side updates perimeter and area correctly*


In order to accomplish our requirement, we will make perimeter and area attributes as properties using property decorator. They will no longer be updated in the __init__ function. This is how we can do it.

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-16-03-28.png)
*update Square class to change perimeter and area based on side value*

And sure enough, our test cases pass
![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-16-10-32.png)

Whenever perimeter and area will be accessed for a object, these property methods will execute and return the correct values. Folks coming from Java can draw an analogy of these @property construct with setter and getters.

---

Lets move towards our new requirement as follows.

> **Requirement**: If perimeter of a square object is updated, the side should get updated automatically based on the new value.

We write a test function first

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-16-22-28.png)
*test to check that updating perimeter also updates side and area correctly*

If you run test at this point, it will fail saying unable to set perimeter attribute. That is fine. I just want to showcase the feedback loop/workflow you should get used to. 

**Write Test => Fail Test => Write code => Pass Test**

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-16-24-37.png)
![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-16-24-56.png)
*test case fails*

In order to update perimeter, we can just write a property.setter for perimeter property

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-16-28-08.png)
*update side according to perimeter's new value*

Now if you run the test cases, they'll pass!

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-16-29-12.png)
*test cases pass*

---

This next requirement is pretty small and closely related to the previous one.

> **Requirement**: You can't set area attribute explicitly

Honestly, there is not much to be done here. Not writing setter for area property will itself prevent any explicit update to area attribute.

What we can do is write a quick test to check that we aren't able to set area.

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-17-00-55.png)
*setting area should raise AttributeError*

Now run the tests one more and make sure they pass

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-17-01-50.png)


---

This next requirement is pretty small and closely related to the previous one.

> **Requirement**: You cannot have negative side attribute for a square object

While this may seem like a small change, we need to be careful of its implications. Worry not, for we will smartly handle this logic in our existing code making minimum changes. I'm a strong propoent of writing less but efficient code when possible. My manager used this quip all too often and I agree wholeheartedly.

> Untested code is not really code. No code is the best kind of code.

Sorry. I digress :)

So for this requirement, we are supposed to make sure setting negative side attribute is not allowed. By virtue of this, we'll need to make sure of a few things

1. Side attribute cannot be negative. This applies both while object creation as well as explicitly setting.
2. You cannot explicitly set perimeter attribute as negative value. We don't need to worry about explicitly setting area attribute as we have not allowed that in the first place(Check the previous requirement)

As always we will first write a test function that matches above understanding.

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-17-21-35.png)
*test function for this final requirement - side cannot be negative*

If you try to run test suite, you will notice that it is currently failing for this particular test ie it is allowing for an object with negative side to be created. Instead, it should ideally throw the error as specified in our test function. We will fix this soon.

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-17-24-42.png)
![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-17-25-45.png)
*fails the test_negative_side_not_allowed function*

We know our requirement. We wrote and failed the test case. Good job! Now it's time to write the code.

We don't want to allow negative side attribute to be set. We will use some property magic and a setter/getter for our side attribute.

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-17-39-40.png)
*changes to prohibit negative side attribute*

There are a couple of important things to note here.

1. Assigning **self.side=side** in init function calls the side setter automatically as the object has been already created by that time.
2. We are using an encapsulated attribute '_side' which is only for the sake of internal implementation. It won't be exposed outside of this class. But we use it save the actual side value. The underscore '_' implies this is a non-public attribute and shoulnd't be accessed outside.
3. We never fiddle with _side anywhere except the specific side setter/getter property methods. No changes to the __init__ method either - it will set side attribute which will then call side property setter
4. We did minimal code changes. For example, you might think if we set perimiter attribute we need to handle it for negative use case. But if you look carefully, perimeter sets side, which in turn calls the side setter.

I implore you to take a look at [property](https://docs.python.org/3/howto/descriptor.html#properties) in Python. It is such a powerful tool to add to your toolbox.

Having written the code for our Class Square, not lets run our test suite one last time.

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-17-56-46.png)
*Voila! All our test cases pass!*

For your reference, I am pasting the screenshots of final version of our Square Class(square.py) as well as the testing module for the same(test_square.py).

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-18-12-09.png)
*Final version of square.py*

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests-part-2/2021-11-04-18-13-09.png)
*Final version of test_square.py*

---

There was a time at the beginning of my career when I shied away from writing detailed tests. They don't really teach its importance in schools and I wonder why. I feel it should be as natural as other tools/principles at a developers disposal - like git, SOLID, YAGNI.

I lucked out in that I came across a manager who really forced us to rigorously(and blindly) embrace Test-Driven-Development. Today I'm certainly happy that I did. 
Believe it or not, we had hooks that didn't allow us to deploy PRs unless we wrote one extra test. That's how we improved coverage for one of the old projects that we found ourselves working on - and it didn't have ANY tests in the beginning. 

Especially if you are joining a new team that's already working on an ongoing project, I would highly recommend:

1. Go through existing test converage. Tests are a great way to navigate large codebases.
2. Start writing tests that you feel are missing, no matter how trivial. You will solidify your understanding of the new system/projects much earlier than you'd anticipate.


I hope this couple of articles help you understnd basic OOPs at the backdrop of TDD. Going forward, I plan on writing more often and gradually advance on to difficult to understand topics using simple examples.

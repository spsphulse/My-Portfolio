---
published: true
layout: post
title: Evolving Code through OOPs and tests - Part 1
---


One of the easiest ways to guage developer proficiency(in any language) is to read their code and see if it's structured well in terms of classes...and more importantly, tests! 

I was fortunate enough to have a Manager early on in my career who emphasized testing so much that it was borderline brutal. To the very extreme. He wouldn't let me write even a line of code before committing the test first. No matter how small or trivial the change would be. All props to him, I embraced TDD early on and have become a strong propoent of the same. And I almost thank him virtually every single day when working on large codebases.

I profusely follow and encourage devs to mix/mash two types of paradigms:

1. Documentation driven development => specifically if writing RESTful APIs for web servies
2. Test driven testing              => well use this everywhere you can

Most beginner developers who have completed a MOOC online or read through a basic Python book mention their frustration with not being able to think in terms of OOPs and TDD. Through a series of posts, I would like to help them onboard on the OOPs/Testing bandwagon. 

Based on my observations, the biggest gap is not being able to mimic real world scenarios. __It's not much important to see the final end product of a well functioning class. But it's critical to understand how you start out with something really simple and then evolve/refactor as per the changing requirements through the lifecycle of the code.__

In this post, and a few posts following this one, I will be walking through a very simple example. While it may seem trivial, it will help you understand the unerpinnings of OOPs and testing by the end of it. I use Python, Scala and Java and enjoy the perks of both dynamically and statically typed languages. Every developer should learn one strict scripting and one JVM based language. Java is great, but I see myself being drawn to Scala since I write a lot of code for data platforms and just love me some native support for functional programming on Scala.

__All that being said, I will be using Python( and unittest framework) to work through our example as I want to keep this beginner dev friendly. Note that the constructs we will go through are language agnostic. I intend to help you develop mental model for OOPs and TDD, not recommend one language over another. For the sake of this series, it should suffice to just read through basic starter pages for Python and unittest framework.__

---

Alright then! With the context set up, let us get our first set of requirement and start writing some code.

> **Requirement**: You need to develop a class for square(ie the shape). It will have side, area and parameter attributes

Sounds so easy. But trust me this simple example will help build the foundation for much complex OOPs/TDD concepts coming in later posts. Hope you will stick around.

Right off the bat, I know I will create a class which accepts side(provided while creating object instance) and then derive other attributes(area and perimeter).

Let's write a stub for this class and immediately proceed to writing unit tests.. Yes.. even before we get to writing the code for our Square class. Please note I have shown author information just to show how it's done. It's automatically populated using a snipped I have saved . I will drop this later in interest of code brevity.


![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-10-05-20.png)
***square.py**  -   Stub/Mock class for Square. We will later add to this*

As you can see above, I didn't even bother to write the any substantial code inside our Square class. Because we are going to be writing tests first. Fail those tests. And then write code piece by piece and see that our tests start passing.

Head on over to our test module (test_square.py). Without thinking too much, I can straight away tell that there will be three basic tests - for side, area and perimieter as those are our core attributes for the square class. Lets write those down.


These are really simple tests.
1. If we create a Square object with side 8, it's side attribute should be equal to... 8
2. If we create a Square object with side 3, it's perimeter attribute should be equal to... 12
3. If we create a Square object with side 3, it's area attribute should be equal to... 9

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-11-21-11.png)
***test_square.py**  -   First three tests based on side, area and perimeter*

All I am doing here is checking that the object attributes(once they are created) will match the manually provided values.

Let's go ahead and run our test_square module. Of course, all the tests will fail since we haven't really written any real code for the Squre class

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-11-23-21.png)
***output of test_square.py**  -   All three tests failed :(*

Time to fix this! We will write the most basic constructor for the object. This __init__ constructor takes side as and arguement and then assigns side attribute. It also sets the values for attributes for area and perimeter using basic geometric formulae. Once that is done we will run our tests once again.

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-11-25-33.png)
*Implemented basic constructor for the square object*

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-11-26-30.png)
*Our three tests now pass!!*

---

Okay with the base for our code working, now let's incorporate some requirement changes.

> **Requirement**: printing square objects should diplay something meaningful

Right now, if you try to print a square object, it will print just an object reference. 

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-11-55-56.png)

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-11-56-36.png)
*calling print on our object just prints object reference*

We want it to print something meaningful...lets say *Square(side)*. Go ahead and fix that using a custom __repr__

Write the corresponding test for repr function first and then define a __repr__ function for our Square class

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-08-22.png)
*test for repr in test_square.py*

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-06-40.png)
*write a repr function for square class*

Now run the tests and make sure they pass

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-13-05.png)

If you create an object of Square class and print it, now it provides somee meaningful display

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-14-29.png)

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-14-02.png)
*now it prints meaningful based on our custom repr function*

---

Lets move on to our next requirement

> **Requirement**: If no side is provided while creating square object, side should be set to 1 by default

For a change as trivial as this, you may choose to skip the test honestly. I've noticed this varies significanltly from team to team. Had it been my aforementioned manager, he would be pleased to see test for even the simplest of changes. I sleep well knowing my test suit is robust. But check with your teammates to set the expectations right.

OKay, we know the drill now. Write a custom test to check for default value if no value is passed while creating the object.
Then we will add an argument to set the default value for side attribute

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-23-41.png)
*function to test the default value*

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-24-33.png)
*__init__ constructor will set side to 1 if it isn't provided explicitly*


We then run the tests and make sure everything passes

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-25-57.png)

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-27-39.png)
*create an object without explicity mentioning side value*

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-12-27-57.png)
*meaningful string representation will be shown in output with default value*

---

I think this is enough for a first post of this series. You can see how TDD(test driven development) can assist you with something as simple as creating an object, setting it's attributes and displaying it meaningfully.

At the end of this post, how does our square.py and test_square.py modules look?

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-15-52-22.png)
*square.py at the end of part 1*

![](/../images/2021-11-04-Evolving-Python-code-through-OOPs-and-tests/2021-11-04-15-53-22.png)
*test_square.py at the end of part 1*


In the upcoming posts we will evolve this code much further with even more requirements thrown our way. Much like a real life project, we will try to incorporate changes as they come.
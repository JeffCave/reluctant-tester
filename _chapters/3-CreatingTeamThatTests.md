---
title: Creating a Team That Tests
---

Based on my experience working with small teams, and observing the 
existing processes, and existing culture, I hope to offer suggestions 
for proceeding that will have a large impact with minimal change. The 
processes I suggest have been drawn from experience at multiple 
organisations, and formalised processes in literature (Test Driven 
Development), but have been tailored as much as possible to the 
cultural environment, and normal processes found among small teams.

Primarily these suggestions revolve around formalising processes that 
usually already exist, and are very close to testing best practices.

## 3.1. Test Driven Development

> The act of writing a unit test is more an act of design 
> than of verification.  It is also more an act of 
> documentation than of verification.  The act of writing a 
> unit test closes a remarkable number of feedback loops, 
> the least of which is the one pertaining to verification 
> of function 
> 
> [Robert C. Martin](http://www.amazon.ca/gp/product/0135974445/ref=as_li_ss_tl?ie=UTF8&amp;camp=15121&amp;creative=390961&amp;creativeASIN=0135974445&amp;linkCode=as2&amp;tag=vius-20)

In the context of small teams and in order to facilitate change and 
adoption, I would recommend that TDD-like practices be implemented 
rather than attempting to implement a formal TDD process system; 
allowing for the a natural adoption of greater TDD practices at a 
later date. Vast volumes of text have been written on Test Driven 
Development and all of it should be read, but adoption and 
understanding takes time and part of the point is to make change in 
small, consumable, increments rather than large, overwhelming, ones. 
In this sense, it is better to take some small steps and alow them to 
grow over time.

In general, Developers should write tests as they develop code. This 
gives them a target to shoot for in their development, as well as 
integrating the regression test development into their process.

Rather than developing all of the tests right from the get-go, the 
first step would be to create a list of test stubs representing the 
list of features that are to be developed against. This list then 
becomes the development objectives for the development request.

```java
//Sample Test Stub

/**
 * &lt;h1>New Feature requested by User&lt;/h1>
 * &lt;p>
 *  Complete description based on user requirements 
 * (see the "Process" section of this document)
 * &lt;/p>
 */
@Test
public void NewFeature(){
    Assert.fail("Not yet implemented");
}
```

This creates a list of tests that need to be fleshed out, and all 
fail. Now the developer has a clear list of unmet objectives that 
need to be fulfilled before closing the ticket. As the developer 
completes the tasks, they should fill in the tests to a point that 
they give a success signal. The task is not considered complete until 
all tests pass.

This mechanism of writing a list of objectives creates a rapid 
feedback mechanism. Since the developer is forced to think out the 
entire task prior to even investigating the problem, they are better 
able to focus their efforts at solving the problem. 

True Test Driven Development (TDD) has strict forms that are to be 
adhered to, but these strict forms are a function of creating a 
theoretic framework. The theoretic framework is supposed to model a 
dynamic and agile environment. Here I am going to describe what I see 
as some best practices based on a foundation of TDD. I have yet to 
meet a team willing to undertake the discipline required for true TDD 
right out of the box, therefore this is TDDlite. 

For a discussion of true TDD practices, I recommend 
[Introduction to Test Driven Development](http://www.agiledata.org/essays/tdd.html) 
by Scott Wambler.


## 3.2. Central Test Database

It is highly advisable that organizations maintain a central database 
of tests. These tests would form a regression library, documenting 
the knowledge acquired by the organization over time. Outside of the 
standard documentation benefits (training, risk assessment, etc.), 
this has several key benefits:

* More thorough testing over time
* Preservation of knowledge
* Training Tool

Over time, as the expectations of users will be forgotten. This can 
occur because people come and go, or simply because people forget 
over time. There is a very low probability that anyone is going to 
remember that there was a key requirement of there being a minimum 
font size of 26px on a component 1 year after the initial development 
was complete. By putting that requirement in a standard checklist 
that people have to validate against, everytime they modify the 
component, it will act as a reminder when someone has to make a 
change in the future. This puts an end to people not understanding 
the full ramifications of changed specifications.

On the flip side, when a developer sees an odd piece of code, they 
may be hesitant to undo it, thinking there must have been a good 
reason to have done that. This level of documentation puts an end to 
the fear of making changes because "It must have done that for a good 
reason". You know the reason, it is in written in the test, and you 
can confidently proceed to change the specification if required.

By putting the documentation in a single location, shared among all 
stakeholders, you significantly reduce the probability that any 
changes to the system will not be replicated through to other 
stakeholders. The maintenance of a list of bug reports, test reports, 
and specification reports means that changes to one set of 
documentation is not replicated through to the other datasets. A 
Central Test Database reduces this by becoming the authoritative data 
repository: everyone is required to keep the central repository up to 
date. This means that changes get replicated out to everybody 
immediately.

## 3.3. Central automated Test server

* Primarily this is to keep the running of tests from interfering 
  with ongoing work people that people are doing
* If their computer is busy running tests, it interferes with their 
  work
* Some tests are general in nature (not the unit tests)
  * these tests can be delegated to central Test Server
* Regression scans are also a good candidate
* RISK: developers see this as "someone else’s responsibility"
  * have already seen this
  * desire for "I write code, someone else does the testing work"
  * mentality arises from testing being viewed as drudgery
  * a separate system would reinforce this mentality
  * public shaming is the general practice to manage this
  * developers that check in code that cause a problem are publicly 
    held accountable, this encourages developers to check their own 
    work, before checking in. (this should be mild and short, not 
    ongoing shaming)

## 3.4. Designate an automation specialist

An Automation Specialist would be a person that sets the standards by 
which the tests will be maintained. 

Any time a process like this needs to be maintained in an ongoing 
fashion, there is a large amount of individual discretion that goes 
into the day to day operations of the system. While this is healthy 
and allows the system to evolve as needed, there needs to be one 
individual that is considered an expert, and can act as arbitrator 
when there is disagreement as to the exact details of implementation. 

This individual should be the person that questions should be 
directed to. While it is important that everyone learn to write 
tests, having a single individual considered the expert forces them 
to accumulate knowledge as people as them questions. They become a 
single repository for all knowledge, while some people have some of 
the picture, having many of the questions filtered through one person 
makes that person the big picture of the problems. This further 
allows them to make better judgement calls in the case of 
disagreement.

This role is similar to and has overlapping responsibilities with the 
Code Auditor role, in that Automated Tests are a form of code, and 
should conform with the overall quality practices of the 
organization. However, it is also distinct from the Code Auditor role 
as it is more focussed on how to implement code as applicable to a 
testing framework. If possible, these responsibilities should be 
shifted to the Quality Auditor role, as maintaining the automated 
tests are more concerned with the descriptiveness of the tests with 
an eye to conformance. 

This role has a pre-requisite of having a working knowledge of 
programming, this allows the individual to engage in becoming an 
expert in this specialized style of programming.

### Key Points

* The person that fills this role should have development 
  experience, and should be familiar with Devlopment 
  guildelines. 
* The Quality Auditor could also fill this role as this is 
  strongly related to the QA position. Likely this role 
  should go to QA to shift workload away from CA, though 
  this will not work if the QA has low development 
  experience, or not available.
* the specialist is only to act as the expert that can 
  answer questions, not write other people’s tests
* This role’s word is law in their domain: on process 
  disagreement, code quality, style guidelines. This domain 
  overlaps with the Code Cop’s domain.

### Risks

The primary risk associated with this is that it will define a 
Testing role again. Testing is the responsibility of all individuals 
on the team, and creating a "specialist" creates a role that is equal 
with "the person that does that stuff". In order to maintain the role 
of "specialist" it is important to reinforce that the role is the 
person that maintains the quality of the tests. The individual in the 
role must refuse to write tests for other people (they should 
continue to write their own), rather offering advice and education to 
people with questions. The first time they offer to do it on behalf 
of another, they have sacrificed their role to being "the person that 
writes the tests".


## 3.5. Make system owners responsible for testing

* creates a sense of responsibility
* remove sense of "someone else’s problem"

### System Health Test

* already begun

### Unit Testing

* performed by developers
* use TDD principles



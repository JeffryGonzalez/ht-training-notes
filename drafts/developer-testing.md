# Developer Testing in CI/CD 

In organizations that have explicit software testing teams, there has always been a lot of gray area and confusion about the thesting responsibilities of the people that write the software (the "developers"), and the people charged with ensuring the software meets the stated requirements (acceptance testing), meets performance requirements, uses consistent metaphors across the rest of the software suite, etc. There will be overlap, for obvious reasons (as a developer, you want to deliver code to the testers that is as "ready to go" as we can make it. We live for those "fist pump" moments when all *our* tests pass, and so do all *their's*)
hat mission is almost always directly related to inc
## What *Kind* of Development Work Are You Doing?

Most of my work is as an *Application Developer*. I am paid to put software in front of human beings, and that software is there to further the "mission" of the company that pays me. Treasing the revenue of the company or reducing liability. (See [The Five Whys](https://kanbanize.com/lean-management/improvement/5-whys-analysis-tool)). 

And that stuff is *important*, and when something is *important* there is always some sort of advantage implied when we can deliver it faster. If a feature is added as a way to increase our revenue, then our ability to ship it faster increases that revenue. Likewise, if it takes us way too long, then a big percentage of the revenue gain will be lost.

While there are probably nearly infinite ways we could categorize the kind of work we do, I find the following list to be a helpful categorization scheme. 

## Application Development

<figure>

![Application Developmet](../../../public/testing/interface-boundary.png)
<figcaption>Figure 1: Developer Poking an Interface</figcaption>
</figure>
For in-house software development teams, this is the *sweet spot*. This is where *they* (the 'business') wants us spending our time because this is where *recognizable benefit* is seen. Getting code in front of users. Frankly, for many of us, this is the place we want to be, too. It's fun to have the "wins", see our users enjoying and benefitting directly from what we do. 

> **Application**: *the action of putting something into operation.* <cite>Google's Oxford Languages Search</cite>

Application Developers are always creating a kind of *interface* for our customers. An interface is a mask over complexity. It's putting a happy face on what is most likely a really complicated, business (domain) specific requirements, processing, and technical stuff that if we unleashed this on our customers they'd run away in horror. 

### Types of Interfaces Created by Application Developers

Application developers work hard to create ways for our customers to accomplish complex things without barfing the complexity on them. We don't give them a login to our relational database, we give them a *translation* of the technical implementation into a more consumable, understandable form. I do not need to know how a car insurance company, for example, comes up with a quote for coverage. I don't even really want to know. And if you did explain it to me, what happens when it changes in the future? I don't want to have to stay up on that. I have my own work to do. Simplify it for me. Give it to me like I'm a five year old. That kind of thing.

So, we, as application developers, usually are creating interfaces for two kinds of users. One is a human being that is going to use the software we deliver directly (user interface), or for other developers (maybe at the same company we work for on other teams or platforms, or for partner companies, etc.) that need access to our *data* or *processing*, but don't need to know how the sausage is made. Those kind of things are Programmatic Interfaces. 



#### User Interfaces (UI)
You know, stuff like web pages, HTML5 Apps, iOS apps, Android Apps, Console driven applications, etc. Software we put in front of our customers with buttons, text boxes, forms, sliders, drag-and drop, all sorts of cool ways (*affordances*) to allow them to accomplish what we need them to accomplish.

#### Programmatic Interfaces (API)

So business says they need this 'thing', and we build an application for it. A bit part of that, in addition to understanding the business needs and requirements, is *technical* knowledge. And that knowledge relies on the types of development below (libraries, frameworks, operating systems). In other words, if everytime we needed an application, we had to build an operating system, reinvent a network protocol, a way to store data, etc. we'd never get anything done. 

### Applications are **FOR** Specific Things


In both cases it's important to be clear that we are talking about *specific* **applications** (we are *applicaton* developers here!). We have a thing we need to user to accomplish. We create a way for them to accomplish it. It is *very* specfic at this level. If you were building a user interface application for a streaming music site, and a customer asked you "how could I use this to play Tetris?" it would be *absurd*. That is not what we built this application *for*!

Applications are *for* specific things. Not for general things. 

This means we can test them pretty darned directly. They serve a *specific* purpose, so we can write tests that guarantee they work for those specific purposes. The whole point of an application interface is to limit the inputs, and make them maps to specific outputs. Super easy to test this stuf. And if it isn't super easy, then maybe you aren't doing application development. You might be doing *library* development, or *framework* development.

## Library Development

> Let's accept the premise, for the sake of argument, that you are an *applicaton developer*. How do you know when you've accidentally slipped into being a *Library Developer*?

So let's say you are on a team cranking out an Angular application. You build a component that displays some data from an API that includes a Social Security Number. You have to display this on the screen, but you want to *mask* it. So you do a little research and find the right way to do it. The right way it should *displayed* [The IRS](https://www.irs.gov/privacy-disclosure/what-are-we-doing-to-protect-taxpayer-privacy) says you should mask all but the last four. So:

```
555-55-5555 => ***-**-5555
```
You write a little helper on your component, or maybe just some Angular template stuff, and you test it (you look at the thing). Great.

Then a couple of days later you have *another* component, and it has the same thing. Displaying SSNs. So you *copy and paste* whatever you did in the first one. You test it. Looks good. Depending on your [refined sense of smell](https://martinfowler.com/bliki/CodeSmell.html), you might want to get rid of the duplication. Definitely after the third or fourth time you do it. So you create a helper function, or maybe an [Anular Pipe](https://angular.io/guide/pipes). You go clean up all your uses of that and use your new pipe. Looks *gorgeous*. You wait for the adoration from your peers when you show them. Whatever.

Days go by. Maybe weeks. Then the new guy on your team lights up your Teams chat saying "Hey, who created that stupid `SsnMaskPipe`! It SUXS!. 

You ask them what is up, and they tell you that they are using it, but sometimes the data from the API they are calling returns 'full' SSNs (like `555-55-5555`), but sometimes the API returns them already 'masked', by just giving the last four (like `5555`), and when he uses your `SsnMaskPipe` with *those* SSNs, it blows up.

He is trying to play Tetris with your application.

So, you kind of 'own' the `SsnMaskPipe` now, it seems. Other people are relying on it. And now you are going to have to add some logic to the thing. There will be at least an `if...else` in there. Maybe you should be slightly preemptive and add some simple validation so nobody bothers you again.

Do you see the move there? It's an important one. You go from a *specific application* to a general one.

That's a good time to add some *unit tests*. Ask the new guy for some *examples* of what they will call your function with and what they'd want it to return. Maybe ask the rest of the team. Write those as a set of tests, and then make them pass.

You are now a *library* programmer. Sort of. You are writing *library* code in an *application* code base.

You are a great programmer, despite what that new guy said. You wrote something useful for you, never intending for it to be useful for others. It worked for you. 

And then to make it more *generally* useful, you beefed it up a bit, surrounded it by tests. Not only are you a good programmer, you are *generous*. 

Everybody in your project is using your `SsnMaskPipe`, and it's not like business is giving you high-fives, but it's nice that you aren't hearing complaints from the compliance people anymore. 

Now, *normally*, we might follow this made up but realistic narrative by saying something like 'So, some other team heard about your great work on the `SsnMaskPipe` and want to use it too! And so you put it in a reusable library for them, blah blah blah.

This is where our narrative might take an unexpected turn. 

> As soon as you create *anything* in your application that is used for more than a *specific application* (something you created for fulfilling the requirements of a feature), and starts to be used in a *general* way within your application by others, it **should be a library**.

In other words, make it a separate module. A separate unit of deployment that your project takes a dependency on. Things like that have different testing requirements. They tend to need more general unit tests because the code has to be more generally reusable. But if you add those tests to your application, then they have to run on *every* commit. 

You and your team (in Angular), but be using a bunch of the Pipes that ship with Angular. `currency`, `date`, etc. Each of them have unit tests. All of *Angular* has unit tests. Do you run *those* every time you commit? Of course not!

This is a software **cohesion** issue. Cohesion means "things go together well", and one of the most important ways things 'go together' for us as software developers is their *rate of change*. 

Think of the rate of change of your Angular applicaton. If you are doing CI/CD, there will be change *at least* every day. The tests you run should be only about those changes. That `SsnMaskPipe`? How often will it change? You are so awesome, frankly, maybe *never*. So why test it everytime some business person says "hey, we have an urgent story to add to this sprint! Can we have that icon on cornflower blue?".

Once you get the gist of this thinking, it changes *everything*. 

All of this is to say that in Application Development, **Unit Tests Are A Code Smell**. Does that mean you shouldn't write them? No. Well, probably not. You'll have some. But after some time, you'll see that they *never fail*. Other stuff fails, the new stuff, but the library type stuff almost never does. That is a sign that the code they are testing is not *cohesive* with the rest of your application code.

Our goal is always to **move fast with confidence**. 


## Systems Programming

## Local Developer Testing

Probably the most common form of "developer testing" is simply *running the darn thing* and poking at it. Clicking buttons, filling out forms, making an HTTP Request using Postman or some other universal client, and manually inspecting the results of that interaction. 

When you have a system you are working on where you can *interact with the application through the interface the user of that system will use* this is a *fantastic* approach. I know sometimes it is looked down upon, especially by the *Test-Driven Development* Pharisees, but so much of our activity at this level is done by a sort of subjective *feel*. When working on the 


> There is a reason that the trajectory of application development has been moving in a weird direction - towards web browser (again), etc. and it has to do with coupling.
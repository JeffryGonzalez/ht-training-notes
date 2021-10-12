# Application Developer Testing

What is an application? UI / API.

You create an application *for* something. The whole point of an application is to *collapse the possible*. 

Libraries *generalize* the possible. "I don't know what you need this for, but here's how you validate a credit card number using the Luhn algorithm.

Frameworks *generalize* the categories of applications. "I don't know what you are building, but if you want an application that runs in a web browser (Angular, React), or you want to build an API that uses HTTP (ASP.NET Core), I can do some of the heavy lifting for you.

So an *application* is a specific way for someone to accomplish something. 'Here is how you can play Tetris!', 'Here is how you can add a vehicle to your policy'.

Applications *aggregate* together libraries and frameworks for a specific use case. 

Here is an API that other developers can use that allow a person to add a vehicle to an insurance policy'. Or, 'here is a web application you can use to add a vehicle to your policy', In both of those applications, there may be the use of shared libraries, but the frameworks might be different. One might use ASP.NET Core, the other might use Angular.

there is an interesting thing here about the 'who' question in relation to the 'instance' of the application.

So a user interface application has contained with in it, implicitly, and explict 'who'. An application knows 'who'. An API knows 'How', but not 'WHO'.

So if we were making a chart of general to specific, the specific end would be a user.

I don't know if it is a difference that needs distinquished or not - but an application is something that runs on behalf of a user. Maybe that is a *good* distinguishing feature.

If you are running an application locally, like Word, it is 'you', for all intents and purposes. Where you can save, where you can print, all that, is dependent on the identity of the person running it.

The more I think about this the more I think it is an important distinction.

I could say:

An **application** is some code that runs for a reason (a purpose) in combination with an identity. "Joe is using the application to add a user to the policy".

So in these terms, an API is an application if it is an authenticated application. 'Joe is submitting a request to the Http API to add a vehicle to his policy'.

An application without an identity is a 'utility' maybe.

So thinking of an application in the context of a MSA


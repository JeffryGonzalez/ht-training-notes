# Containers
When you run applications on servers in production, the safest way is at a minimum one server per application. That way that one application has little opportunity to negatively impact any other applications on that machine. Use up all the memory, the hard drive, access stuff it shouldn’t be accessing, etc. That would be awesome. And with unlimited resources, maybe a good way to do it. But *wow* would that be expensive!

## Virtual Machines

Another way to do it is to take a single server and run multiple isolated operating systems on it. “Virtual Machines”. However, that isn’t a great use of resources, either. Each of those Virtual Machines (VMs) are *heavy*, and don’t scale well. Imagine your desktop with five copies of Microsoft Word running. Now imagine your desktop with five copies of windows running, each running a single copy of Microsoft word. Kind of like that. 

## Linux Containers
So, the Linux folks figured out a happy medium a while ago. This is something that is baked into the heart of Linux (the “kernel”) called *namespaces*. It’s a way to segment a machine so that software running in different namespaces can be lied to. Each namespace can be told to “report” it has X amount of RAM, Y amount of processor, access to this particular disk mount, etc. To the applications living in a namespace, they are blissfully unaware that they are living in a little glass jar inside of a much bigger universe. They can’t see their neighbors, and they can’t ask for more than they are allotted.

Linux Namespaces are awesome. They aren’t easy. And they don’t really have much in the way of affordances to help developers define and install them. As a developer, you basically deliver your code to the production people, and, if they are into it, they can define a namespace for your app. They will set limits on its resources. It’s considered none of your business really.

If you want, the Wikipedia article is pretty ok about this. [Check it out]([Linux namespaces - Wikipedia](https://en.wikipedia.org/wiki/Linux_namespaces)).

Well, part of the whole DevOps revolution is that this disconnect between the people that write the code (Devs), and the people that make it run reliably and securely (Ops) just doesn’t work in practice. Not well. It served us for a while, maybe, but we’ve outgrown it. What if the Ops people don’t give us the *correct* amount of resources for our application? What if we are sending them new versions really quickly, and they become the bottleneck? The Operations folks have done glorious work over the decades of letting developers live in ignorant bliss about things like security, reliability, dependencies, all that. It turned into a game of virtual Battleship. We’d keep making guesses until we’d sink their carrier, and them blame them!

## Enter Docker 
Docker was created as a way for us to responsibly close that gap. As part of our applications deliverable, we will, in pretty simple terms, define exactly what we, as developers, need for this application to run. What dependencies, what networking access, memory, disk, all that. You do it in a pretty abstract way. Like “I wrote this assuming this can have TCP port 80. And that I can write to a directory called `/etc/mydata`, and I think I can run with about 2 GB of ram, but might need up to 4. And oh, I run on this version of Linux, and need these other things installed. I need some linux utilities, like `curl`, I need NodeJS (or .NET Core 3.1, or Python, or whatever).  And  - oh, the data I need lives on or corporate database, so I’ll need a connection to that.
Now, you could just put this in an email or something. A set of instructions for the Ops people to follow when configuring your namespace.
As you can imagine, this might create a lot of back-and-forth. Lots of Teams meetings. Lots of mistakes.
Docker lets you specify all that, and then actually *run it* in an **exact** replica of how it will run on their machines. Instant feedback! Run your tests against it! Debug into it! Do a tiny bit of load testing on it to see how it behaves. Docker builds a *container image* of your application that you can *run* which means the difference between your environment and the production (or test, or qa, or compliance, or whatever) environment is almost completely eliminated. No more “It runs on my machine!” Problems. Remember, DevOps is about *confidence*. What can we do to increase the confidence that the code we produce will work as designed when it gets in front of a user. This is *huge*.

Somewhere near the end of your developer pipeline, the instructions you wrote on how the container image should be constructed (the `Dockerfile` will be used to build the container image that will move to the next environment. It would be pretty creepy if you hadn’t actually *tried it* before you shipped it, wouldn’t it. 

**You must be able to run and verify your container images locally if you are going to sign off on them going to the next environment**.

So, let’s be responsible. If you aren’t running your containers locally, running your tests against your containerized applications, verifying them for yourself in front of God and your team, you’ve kind of short-circuited the whole inspiration behind all of this stuff. You are still saying “hey, running code in production is your problem, not mine!” And you are missing the point.

And if you are creating something that you feel you can't run locally in a container in a meaningful way because it is "too big", or "has too many dependencies on outside things" - that are all *huge* red flags about your architecture that are being shown to you. Don't sweep this feedback under the rug!

## Kubernetes and Containers
Modern Kubernetes runs containers. For a long time it used the same Docker application we use on our developer machines to run those containers. But Docker not only runs containers. It builds them. It has an API. It works as a mini container registry on your machine. In other words, Docker is kind of a *monolith*. Kubernetes only needs to run them. So now it just has the stuff to pull down those container images from your registry at the end of your pipeline and run them.

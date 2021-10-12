# List of Terms and Buzzwords

## Cloud Native

Official definition of this term from the [Cloud Native Foundation](https://github.com/cncf/foundation/blob/master/charter.md)

>Cloud native technologies empower organizations to build and run scalable applications in modern, dynamic environments such as public, private, and hybrid clouds. Containers, service meshes, microservices, immutable infrastructure, and declarative APIs exemplify this approach.
>These techniques enable loosely coupled systems that are resilient, manageable, and observable. Combined with robust automation, they allow engineers to make high-impact changes frequently and predictably with minimal toil.

Despite what some think, this * does **not** * mean that your software runs "in the cloud", necessarily. It means *we co-opt the technology and practices used by the cloud providers (Amazon, Google, Azure, etc.)* for reliably running **lots** of applications in a secure way as efficiently as possible.

By using Cloud Native technologies we have *choices*. We can run our software on our own infrastructure, or extend or entirely move it into the "cloud".

By leaning into Cloud Native as a guiding principle, we are saying "We will build our software system *so that* we have the option of running them wherever it is best to run them (on prem, on rented machines, both)"

Using non-cloud native technology means we do not have the *choice* to run (or extend) our compute and storage into the cloud.

Likewise, using *Cloud Only* technology means that we cannot run the software on prem.

I feel most comfortable staying in "Cloud Native" land because it gives me options. I try not to use non-cloud technologies, nor do I easily decide to use "cloud only" technologies.

## Containers
Self-Contained applications that contain their execution environment (operating system, etc.) to ensure as much as we can that the code that runs in one environment will also run in another. 

Containers need software to *create* them, and software to *run* them.

They are created out of "layers", which are carefully designed so that the parts of our application most likely to change (usually our application) can be updated without having to reship the entire container.

The unit of deployment is technically a *container image*, and a *running* container image is a *container*.

TL;DR - A way to ship software reliably from environment to environment that virtually eliminates the category of software deployment issues referred to as "I don't know what's wrong, it ran on my machine!".

Containers are published to a *registry*, and can be *pulled* from a registry. 

## Docker
A company. [Docker.com](https://docker.com).

They create a tool (Docker) that can build containers and run them. They also have a bunch of related tools to make software development and running container images more awesome, like Docker-Compose.

Docker builds containers that follow a specification [Open Container Initiative](https://opencontainers.org/). These containers can and do run on software other than Docker. [Containerd](https://containerd.io/) is an example.

## Orchestration

Containers run a machine that has the software to run containers (Linux only). But applications span many machines. So applications running in a container need access to storage, to the network, to all sorts of stuff.

You know who has a vested interest in running lots of applications as cheaply and efficiently as possible? The companies that sell us server space, like Amazon AWS, Google, and Azure.

Software that helps manage the deployment, monitoring, availability, security, etc. of container applications running on lots of machines is "orchestration" software.

Examples of this sort of software include:
- Apache Mesos
- OpenStack
- Kubernetes

It is pretty darned clear at this point that Kubernetes has "won" the contest for orchestration.


## Kubernetes

Kubernetes (pronounced *Koo Ber Net Eees*) is an cloud native orchestration suite originally developed at Google for managing their applications and infrastructure. Kubernetes is sometimes abbreviated as K8S ("Kates"), because there are 8 letters between the K and the S. Cute. 

Kubernetes gets is name from the same Greek word we use for "Cybernetics". The following definition of Cybernetics from [Wikipedia](https://en.wikipedia.org/wiki/Cybernetics) is interesting:

> Cybernetics is ... [an] approach concerned with regulatory and purposive systems—their structures, constraints, and possibilities. The core concept of the discipline is *circular causality or feedback* —that is, where the outcomes of actions are taken as inputs for further action. Cybernetics is concerned with such processes however they are embodied, including in ecological, technological, biological, cognitive, and social systems, and in the context of practical activities such as designing, learning, managing, conversation, and the practice of cybernetics itself. 

The much touted term "Feedback Loop" was popularized by Norbert Weiner, who was an original proponent of the study of Cybernetics.

Kubernetes is [Open Source software](https://kubernetes.io/).  

It is a *clustering* tool. It allows you to specify a bunch of servers (or VMs) as *nodes*. The nodes are treated as "one" entity, and there is an API you use to give work to the cluster. In effect this means that as a developer, deploying an application to a Kubernetes cluster with 100 nodes is exactly the same as a cluster with just one or two. You give it the "work" to do, expressed as the "desired state of your application", and that work is *scheduled*. The cluster finds the nodes best prepared to run your application, deploys the containers, etc. 

Your applications are deployed and the real magic is in how Kubernetes monitors the running applications to make sure that they continually match your desired state. If they don't (for example, a container crashes, or a node blows up), it automatically fixes it. Your desired state is the supreme law of the land and you have legions of cybernetic robot servers making sure your wishes are granted, and STAY that way. If only life were that easy.

## OpenShift

As was mentioned, Kubernetes is Open Source software. Just free. Go download it! 

However, because Kubernetes runs in many different environments (like Amazon's AWS, Azure, etc.) Kubernetes makes some promises it can't quite keep. For example, an application deployed to a K8S cluster can say "Hey, what I *desire* here is several instances of this service, and put a load balancer in front of it to evenly distribute the work between each of these!" Well, Kubernetes promises a load balancer, but they don't *give* you one. When you install it, you have to say "oh, here's the load balancer we use around here!" (for example, Amazon's has a load balancer that has a meter on it. If you ask for it, you can have it. But you will pay. C.R.E.A.M.).

So, when you go to install Kubernetes you have to fill in some of these gaps, like load balancers, ingress services, monitoring, etc.

Also, it's Open Source. Fantastic. But who are you going to hire to come fix it when it breaks? Can we get an SLA with that, please? Etc. Etc.

RedHat offers a commercial distribution of Kubernetes that fills in some of the gaps, makes it easier to install and get up and running, and they support it. That is OpenShift.

The OpenShift API is compatible with Kubernetes. Applications written for Kubernetes can be deployed to OpenShift because OpenShift *is* Kubernetes.

It's awesome. We have support, we can sleep better, and has some pretty rad tools along with it.

But `Kubernetes == OpenShift` is pretty true.




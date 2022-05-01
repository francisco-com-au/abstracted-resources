# K8S Abstracted Resources
Goal is to enable Platform Engineers to *easily* create custom resources and operators.

![alt text](https://github.com/francisco-com-au/abstracted-resources/blob/main/design/images/meme.jpg?raw=true)

# Motivation
## The current state of the cloud
As the world moves towards cloud native applications we are seeing an increasing need to set controls, policies and SRE practices. As a result the cloud is being locked down by Platform Engineers to make it more "corporate friendly".

The downside is that we have taken away usability. Developers can no longer self-serve because the cloud is now super hard to use.

Let's look at this example:
As a Developer I need to create a Pub/Sub topic and a subscription for the app I'm working on. I can't create it from the GCP UI anymore because my company implements SRE and does infra as code, so I need to find the right repo where the infra lives and create a Pull Request. But first I need to define that the topic will exist in some Asset Registry, that's another PR. Then I need to give it an identity and create a bunch of IAM bindings, another PR. Finally I need to create the topic itself. These resources may be written in Terraform HCL which I may not know (and I shouldn't have to) or as a Crossplane or Google KCC resource defined in YAML that only god knows what the schema is, so I will probably copy and paste someone else's code. I will probably make a mistake and hopefully the reviewer will catch it, but most likeley it will be merged and then fail.

This kills development velocity, tasks your Platform team to support the Developers team and makes the learning curve more steep.

Now imagine this use case: "I want to write a web app and expose it on a domain". You need to: create a repo, integrate it with some CI/CD, deploy it somewhere, open up an ingress, create and update SSL certs, configure DNS, configure IAM for everything, set up monitoring, logging and alerting, etc. Makes you cringe, doesn't it?


## Where we want to go
- Platform Engineers will still lock down the cloud and inject their opinions, this is good for many reasons. But they also need to provide an interface that is easy to consume. You can't expect your Developers to understand the complexity of your platform.
- Platform Engineers will create *Abstracted Resources* that everyone can consume *easily*.
- Developers will consume those Abstracted Resources from code if they feel comfortable or from a *nice* UI.

This is no revolutionary idea. I'm not claiming to be a visionary. But the thing is that we just don't do it. Why? Because it's too hard. It's too hard for Platform Engineers to create these offerings. And this is where we come in. We want to make it easy for Platform Engineers to create abstractions to then make it easy for Developers to do their job.

# Proposed solution
## Design principles
- Keep it simple, stupid
- Minimize the learning curve by:
    - not reinventing the wheel
    - leveraging existing technologies where possible

## A word on Kubernetes
Kubernetes has become the lowest common denominator in the industry. It orchestrates containers but it can also manage infrastructure. It has a nice API (some may say that it is the one API to rule them all!). There is a lot of open source tooling built to play nice with Kubernetes. We want to build our solution as an extension of Kubernetes as well.

You should be able to install this with a simple `kubectl apply -f ...`

## Design
Detailed design documents are in the `./design` folder.
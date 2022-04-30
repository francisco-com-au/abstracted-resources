# Design
## Design principles
- Keep it simple, stupid
- Minimize the learning curve by:
    - not reinventing the wheel
    - leveraging existing technologies where possible

## High level user experience
![alt text](https://github.com/francisco-com-au/abstracted-resources/blob/main/design/images/high_level_draft.jpg?raw=true)

## Abstracted Resource
Think of it as a `Deployment`. It has its own `apiVersion`, `kind` and `schema` (spec). You can interact with it using `kubectl`, check the status, create it and delete it.

The deployment itself doesn't do anything, but there is a `Controller` watching deployment objects that creates a `replicaset`, `volumes` and other resources to satisfy the desired state of the deployment. So the deployment is an `abstraction` on top of other resources. The replicaset is also an abstraction on top of `pods`. The pod is an abstraction on top of a `container`. You get the idea.

Wouldn't it be nice to be able to create your own abstractions? That is what an `Abstracted Resource` is. Just like `Deployments` it has its own `apiVersion` and `kind`. Let's say you create a `SuperDeployment` abstracted resource. You can interact with it by doing `kubectl get SuperDeployment --all-namespaces`

## Abstracted Resource Definition
A Platform Engineer defines an *abstraction* by creating an "Abstracted Resource Definition" (ARD for short).

This tells the operator:
- what the `apiVersion` + `kind` of the new Abstracted Resource will be
- what the schema (`spec`) of the Abstracted Resource will be
- what to do when a new Abstracted Resource is created

Upon creation of an ARD, the operator will:
- create a CRD for the Abstracted Resource (so users can create them using `kubectl apply`)
- watch the creation of resources of that `apiVersion` + `kind`

# Existing tooling
## Crossplane
If you are familiar with Crossplane Compositions, you may think that this is very similar, and you would be right. This attempts to solve the same problem but tackling a couple of limitations with Crossplane, such as:
- a Crossplane XRD won't let you iterate over a list (by design)
- it has its own rendering engine
- it's hard to create other Kubernetes objects (you need a Kubernetes provider and the syntax is verbose)

## Kustomize
You could kind of achieve something similar with Kustomize but there are several limitations:
- very verbose
- can't iterate over a list
- variables substitutions are hard

## Helm
This is probably the closest one. You could roughly make this approximation:
- Abstracted Resource Definition = Helm Chart
- Abstracted Resource Instance = Helm Values
- Resources produced on the back of the AR = Helm templates rendered with the values

Helm has a great template engine but there are a couple of downsides:
- Charts need to be hosted in some repository (adds external components)
- There is no strong validation of the Values (prone to render-time errors due to missing values)

## Our take
We want to take Crossplane's XRD concept and implement it using Helm's templating engine. Everything will be hosted on Kubernetes removing the need to host artifacts in external repositories.
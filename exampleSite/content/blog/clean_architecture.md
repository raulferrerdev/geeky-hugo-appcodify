---
title: "What is Clean Architecture?"
description: "What is Clean Architecture and why is so important in the softwrare development."
date: 2018-01-03
categories: ["Architecture"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---

## Choosing a Clean Architecture
What does mean **Clean Architecture**. When we have to develop a new software project, we always think about what architecture we are going to apply, that is, how we are going to structure the different components of said software, what we look for is that these components are as isolated as possible so that they are easily test, scal, or even change the components for another without affecting the rest of the software.
{{<ads1>}}

When we want to choose an architecture for our software we can find several options, calculate the best known are MWC, MVP, MVVM, and architectures such as VIPER o  VIP have recently come out, what they are looking for is the greatest separation of responsibilities within the process These last two are born from the attempt to implement in Mobile development what is known as **Clean architecture**.

## Origin of the concept 'Clean Architecture'
Robert C. Martin first proposed the idea of ["Clean Architecture"](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) in 2012. It is not an architecture, but rather a set of guidelines that, when combined with the SOLID principles, allow us to create software that is tested, resilient, and easy to comprehend. According to this idea, for an architecture to be considered "clean" it must have, at least, these three layers: a domain layer, a presentation layer, and a data layer.
{{< image src="images/posts/clean_architecture_1.png" alt="Clean Architecture schema">}}

## Layers
### Domain layer
The **Domain layer** is the central component of this design. Has no external dependencies, so it's easy to test. This layer contains the business logic of the program and encompasses various software components:

* *Use cases*. Use cases are those software components that define and implement business logic. It is in charge of managing the flow of input/output information with the entities. 
* *Entities*. They are simple data structures (although they may contain some methods), which contain the business rules (a claim that places some sort of restriction on a certain aspect).
 such as the use cases (or interactors), the entities (models) and the interfaces that allow communication with the repositories.
* *Interfaces*. The interfaces contain the definition of the methods that must later be implemented in the repositories. Said repositories will be in charge of obtaining information from databases, servers...

### Presentation layer

The **Presentation layer** not only includes those elements that display information to the user or that are with which the user interacts, but also others such as the ViewModel (used in the MVVM architecture) or the Presenter (found in architectures such as MVP, VIPER or VIP). This layer only have dependencies with de **Domain layer**.

### Data layer

The **Data layer** contains the Repositories (which implement the methods of the Interfaces) and is in charge of accessing databases, servers...

> **Dependency rule.** For a correct implementation of the 'Clean Architecture' concept, the so-called 'Dependency rule' must be applied, according to which, the inner layers must not have evidence of the existence of the upper layers. Therefore, any variable or method of an outer layer cannot appear in a more inner layer.

## What is achieved by applying a 'Clean architecture'?

If when developing our software we try to follow the principles of a 'Clean Architecture', we will achieve, among other things, that our software is:

* Testable, we already have the business logic isolated in its own layer and without dependencies on external layers.
* Independent of the UI, since it is in the outermost layer and only shows the data that the Presenter passes to it. We can say that it would be a 'dumb' view that does not follow logic.
* Independent of the data sources, because they are in the outermost layer and we can change them without affecting the business logic, which is inside. In reality, the business logic must be independent of everything that surrounds it, in order to be able to change any external element that affects it.
* Not relying specific frameworks, which allows us to change them without significantly affecting (that is, with the minimum possible changes) to the rest of the code.ç
{{<ads2>}}

## Conclusion
More than an architecture, **Clean architecture** refers to a series of rules or ideas, that is, a philosophy in the background, what it does not know how to allow is to develop software that is much more testeable, scalable and resilient to change.

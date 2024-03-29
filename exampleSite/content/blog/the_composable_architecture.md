---
title: "The Composable Architecture (TCA) in iOS apps."
description: "Learn how to use The Composable Architecture (TCA) in iOS apps."
date: 2023-04-15
categories: ["Swift", "Architecture"]
tags: ["Development"]
draft: true
---

## What's the The Composable Architecture (TCA)
**The Composable Architecture (TCA)** is a modern software architecture pattern that allows developing scalable and maintainable iOS applications. TCA is built upon the principles of functional programming and provides a clear separation of concerns between the different components of an application. It was developed by [Point-Free](https://www.pointfree.co/collections/composable-architecture), a online video series on Swift and functional programming.
{{<ads1>}}

At its core, **The Composable Architecture (TCA)** is all about breaking down a complex app into small, reusable pieces that can be composed together. This approach makes it easier to reason about an app's behavior and makes it more modular and testable.

The central idea behind **The Composable Architecture (TCA)** is that every part of an app should be a pure function of its inputs. This means that given the same inputs, the output should always be the same. In TCA, the inputs are represented by the app's state and actions, and the output is the updated state.

## TCA components

### State
The **state** in TCA reflects the app's current state. It is a *struct* that holds all of the information required to describe the app's UI and behavior. Because the **state** is immutable, each time it changes, a new copy of the **state** is generated. The **state** can only be changed by a *reducer function*, this is why makes reasoning about an app's behavior simpler.

In a ToDo app, for example, the state would usually include a list of todo items as well as their current status. (e.g., completed, pending, deleted). Other UI-related state, such as whether a specific screen is presently displayed, may also be included in the state.

```swift
struct TodoAppState: Equatable {
    var todoItems: [TodoItem]
    var isEditing: Bool
}

struct TodoItem: Equatable, Identifiable {
    let id: UUID
    var text: String
    var isCompleted: Bool
}
```

### Actions
**Actions** describe the events that can occur in an app. User interactions, network requests, or any other event that can cause the state to alter are examples. **Actions** are straightforward structs that hold all of the information required to update the state.

Adding a new todo item, marking a todo item as finished, deleting a todo item, or navigating to a different screen are all examples of actions in our examplle todo app. Each move would be represented by a struct containing any pertinent data. (e.g., the text of the new todo item, the id of the item to be deleted).


```swift
enum TodoAppAction {
    case addTodo
    case removeTodo(IndexSet)
    case toggleCompleted(IndexSet)
    case setEditing(Bool)
    case updateTodoText(IndexSet, String)
}
```
### Reducers

**Reducers** are functions in TCA that take the existing state and an action and return a new state. The **reducer** is in charge of updating the state as a result of an operation. The reducer function should be a pure function, which means it should rely solely on its inputs and should have no secondary effects.

The reducer function in our todo app would take the current state and an action as input and return a new state that represents the change requested by the action. If the user adds a new todo item, for example, the **reducer** may generate a new state with the new item added to the list. If the user marks a todo item as completed, the **reducer** might update the status of the corresponding item in the list.

```swift
let todoAppReducer = Reducer<TodoAppState, TodoAppAction, TodoAppEnvironment> { state, action, environment in
    switch action {
    case .addTodo:
        let newTodo = TodoItem(id: UUID(), text: "", isCompleted: false)
        state.todoItems.append(newTodo)
        return .none
        
    case let .removeTodo(indices):
        state.todoItems.remove(atOffsets: indices)
        return .none
        
    case let .toggleCompleted(indices):
        state.todoItems
            .enumerated()
            .filter { indices.contains($0.offset) }
            .forEach { index, _ in
                state.todoItems[index].isCompleted.toggle()
            }
        return .none
        
    case let .setEditing(isEditing):
        state.isEditing = isEditing
        return .none
        
    case let .updateTodoText(indices, newText):
        state.todoItems
            .enumerated()
            .filter { indices.contains($0.offset) }
            .forEach { index, _ in
                state.todoItems[index].text = newText
            }
        return .none
    }
}
```
### Environment
In TCA, **environment** is a *struct* that contains all of the dependencies that an app requires to operate. Network clients, databases, and other third-party tools are examples of this. It is easier to test and maintain the app if these dependencies are kept separate from the remainder of the app.

In our example, the environment may include a database service for persisting todo items, as well as any other services or APIs with which the app must communicate. The environment would be used to encapsulate and make these dependencies available to the remainder of the app.

```swift
struct TodoAppEnvironment {
    var mainQueue: AnySchedulerOf<DispatchQueue>
}
```
{{<ads2>}}

### Store

The **store** functions as a state machine, receiving **actions** from the user or the system and updating the todo app's **state** appropriately. The **state** could be any data type that reflects the current **state** of the todo app, such as a list of todo items or a boolean flag showing whether a modal is presently displayed.

When an **action** is dispatched to the **store**, it first passes through any middleware that has been specified for the store. Middleware can be used to intercept and modify activities before they are processed by the Store. Middleware, for example, can be used to log actions, handle errors, or dispatch additional actions depending on the original action.


The **action** is passed to the store's **reducer** after the middleware has handled it. The **reducer** is in charge of updating the **state** of the todo app depending on the dispatched **action**. The **reducer** is a pure function that accepts the current state and the dispatched **action** and returns a new **state** reflecting the action's changes.

The **reducer** can return an effect in addition to updating the status of the todo app. Effects allow you to do things like make network requests or communicate with the device's file system. Effects run outside of the reducer and can return extra actions to the store.

```swift
let todoAppStore = Store(
    initialState: TodoAppState(todoItems: [], isEditing: false),
    reducer: todoAppReducer,
    environment: TodoAppEnvironment(mainQueue: DispatchQueue.main.eraseToAnyScheduler())
)
```

store.send(.addItem(text: "Buy groceries"))
store.send(.addItem(text: "Do laundry"))
store.send(.toggleCompleted(id: <id of a todo item>))
store.send(.deleteItem(id: <id of a todo item>))
store.send(.toggleEditing(true))
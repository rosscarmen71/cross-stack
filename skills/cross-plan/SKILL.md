---
name: cross-plan
description: create a plan from the initial request by first interrogating the user to concretely define what it is that they want, only then moving on to defining how to close the differance btween the current state and the definde outcome. The input is an initial request. The output is a plan breaking down the request into parts with at detailed deom of the indended final outcome and state at each stage of development, a demo of what doen liiks like, i detailed descripiton of all requiorments and desitions about how to acheve it, documentation of why for what and how, and i definition of the stratage to be used it unexpected problems arise during operation.
disable-model-invocation: true
---

# Cross Plan

create a plan from the initial request by first interrogating the user to concretely define what it is that they want, only then moving on to defining how to close the differance btween the current state and the definde outcome.  Prioritizing creating visual or state based confirmation of what the final product should look like. After the final state is thoroughly defined prioritizing a visual confirmation if sensible, then move onto defining in detail every possible choice about how to implement the needed changes. Referred to user to get clarification whenever possible and whenever there is any potential decision that needs to be made about what or how. Should also have a very clear definition of why for both why the user wants to change why the user wants the specific output and why the user wants to do things in a specific way you should store this context of how what and why to keep in mind and help make decisions and verification later. The entire process prioritize visual confirmation and extensive verification layers and checks for the output whenever possible. Should also prioritize breaking large tasks down into smaller modular outputs that can have independent definitions of when that modular fix is done. All sub tasks should remain in memory, though in case a future task reverses the progress of a previous task.



## 1. Define The `What`

Come to an agreed understanding with the user about exactly what they want to accomplish.

### 1.1. Clear Up Vagueness

**Interrogate Initial What.** Your goal in this step is to resolve a vage definition of `what` the user wants for the request. Input the request from the user to the **`/grill-what`** skill (omit any parts of the request that only discuss `how`). 

### 1.2. Create Global Demo

**Demonstrate Understanding.** Demonstrate to the user what your current understanding of what they want is by showing them a demo of what the final final result will look like and how it will behave. Match the output description of `what` heeds to be done from `/grill-what` in step 1.1 to a playbook and open its steps at ./playbooks-demos/<title>.md, with the title in kebab case. 

You should have a very significant preference for selecting visual playbooks. Only select the `spec_doc.md` playbook if all other playbooks are impractical. 

Copy the slected playboks `Creation Steps` into the todolist tool first. Run ceach of those steps from the playbook now to create the demo. 

Run the `Show User Steps` in the playbook to show the user the demo. 

### 1.3. Revision Loop

**Revise The Main Demo.** Explicitly ask the user if they are satisfyed with the demo from step 1.2 wich is indened to show the final state when the request is satisfied.

**If the user is not satisfyed.** 

**Once the user is satisfyed.**

### 1.4. Build Out verifictaion Layer




## 2. Identify The Delta

## 3. Scope The `How`

## 4. Create The Roadmap

## 5. Outline The Emergency Strategy

## 6. Finalize
# Creating a Tutorial

This repository contains a template project for a DS12 tutorial or lab. Please use this as a reference when creating your own documentation, source code and sbt files. Note that `classname` should be one of: `scala`, `models` or `methods`.

When creating your project you should make a README_ANSWERS first, then remove the answers and use the remainder as your README. You should also do this with your source code, i.e. create a directory hierarchy for your answers (`code/tutorialAnswers/src/main/scala/tutorialAnswers/lecture1`) and then strip the answers out and create a directory hierarchy for your skeleton code (`code/tutorial/src/main/scala/tutorial/lecture1`). Please reference the included tutorial for a concrete example.

The project should (obviously) be distributed to residents without the answers. After they have had a chance to complete the tutorial you can re-distribute the entire project. 

Instructions throughout the tutorial should be written in clear, concise, and imperative language.

The remainder of this README consists of general style conventions. 

## Part 0

Your project should be broken up into mulitple Parts. Use Part 0 to list the tutorial objectives. For example: 

In this tutorial we aim to review, define or explain
 
* inheritance
* Multiple inheritance and the "diamond problem"
* composition / mix-ins
* implicit conversions
* type classes, and
* implicit parameters


### Task (0a): Task Name

A tutorial should have roughly ten Tasks (a few more or less is ok depending on Task difficulty), labs can have more. Each Task should have some deliverable source code (often in the form of a function implementation). Tasks in Part 0 should generally correspond to project setup or data acquisition.

### Task (0b): `functionName`

A Part need not have Tasks, however each Task should have a descriptive name. The name could describe the problem, or refer to an unimplemented function in the source code.

---

## Part 1: [Useful Wikipedia/SO link](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))

`code/tutorials/src/main/scala/tutorials/lecture3/Hierarchy.scala`

### (1a): Subsection

You can optionally break out your Part into subsections like this. If you do then the subsections should correspond to the tasks with the same label (e.g. 1a). Also please note the reference to the corresponding `.scala` file above.

### Task (1a): Task Name

This is a Task corresponding to the Subsection above. Please note that Tasks should be enumerated starting at `a` even if there is only one Task in a Part.

### (1b): Subsection

If you do break your Part into Subsections, there should be at least two of them.

---

## Resources

Include other relevant references (blog/SO posts, articles, books, etc) here.
Dependency Tool for D
=====================

[![Build Status](https://github.com/funkwerk-mobility/depend/workflows/CI/badge.svg)](https://github.com/funkwerk-mobility/depend/actions?query=workflow%3ACI)

The `depend` tool helps to visualize import dependencies.
Moreover, it allows to check them against a UML model of target dependencies.

Example
-------

Using the `depend` tool on the code of the popular [vibe.d][] repository gives an overview of the package dependencies:
[package diagram](../media/vibe.png?raw=true)

Usage
-----

Use [`dub`][] to build the `depend` tool:

    dub build --build=release

Run the `depend` tool like you run `dmd` (most arguments are just passed to `dmd`).

For example, run the `depend` tool on its own code:

    depend --compiler dmd src/*.d

Consider switching to module dependencies instead of package dependencies:

    depend --detail src/*.d

The output is a package diagram in the [PlantUML][] language:

    @startuml
    package check {}
    package deps {}
    package graph {}
    package imports {}
    package main {}
    package model {}
    package settings {}
    package uml {}

    check ..> model
    deps ..> model
    graph ..> model
    imports ..> model
    main ..> check
    main ..> deps
    main ..> graph
    main ..> imports
    main ..> settings
    main ..> uml
    settings ..> model
    uml ..> model
    @enduml

Copy and paste the output to the [PlantUML Server][] for visualization:

![package diagram](../media/self.png?raw=true)

For checking, specify the target dependencies in the [PlantUML][] language.
For example, create a text file `target.uml`.

Use the `depend` tool to check the import dependencies against the target dependencies:

    depend --detail src/*.d --check target.uml

Then, the check will complain about violations:

    error: unintended dependency main -> deps
    error: unintended dependency main -> graph
    error: unintended dependency main -> uml
    error: unintended dependency uml -> graph

You will also get warnings about cyclic dependencies.

Alternatively, run `dmd` up front and pass the extracted dependencies explicitly:

    dmd -deps=dependencies ...
    depend --deps dependencies

[`dub`]: https://code.dlang.org/
[vibe.d]: https://github.com/vibe-d/vibe.d
[PlantUML]: https://plantuml.com/
[PlantUML Server]: http://www.plantuml.com/plantuml

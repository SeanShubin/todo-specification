# Specification for Scala training sample project
- part of a three project sample todo application
- defines communication protocol between application and persistence

## Sample Projects

### Check out the sample projects

    mkdir training
    cd training
    git clone https://github.com/SeanShubin/todo-application
    git clone https://github.com/SeanShubin/todo-persistence
    git clone https://github.com/SeanShubin/todo-specification

### Set up your environment
- Make sure you can quickly tell if the following tests are green
    - todo-persistence/core/src/test/scala/com/seanshubin/todo/persistence/core/JettyRunnerTest.scala
    - todo-application/core/src/test/scala/com/seanshubin/todo/application/core/JettyRunnerTest.scala

## Run the sample projects

### Install specification to your local maven repository
    cd todo-specification
    mvn install

### Package and run persistence
    cd todo-persistence
    mvn package
    ./run.sh

### Package and run application
    cd todo-application
    mvn package
    ./run.sh

### Navigate to application
- http://localhost:7001

## Priorities
- Meet Customer Need
- Easy To Maintain (same as: Easy To Test)
- Clearly Express Intent
- No Duplicate Code
- Concise As Possible

## Sample design favoring maintainability
- The goal here is to provide a concrete example of a way to manage code complexity in pure scala
- Keep in mind that I am demonstrating how to write code that is easy to maintain, not easy to write
- Code is initially written once, by few, but maintained perpetually, by many
- It takes a great deal of discipline and effort to write maintainable code, but it is not complicated once you know what to do
- Feel free to point out any improvements that make the code easier to maintain

## Design Principles I noticed I was using while developing the sample application
- Structured Design
    - [Loose Coupling](http://www.win.tue.nl/~wstomv/quotes/structured-design.html#6)
    - [High Cohesion](http://www.win.tue.nl/~wstomv/quotes/structured-design.html#7)
- Top Down Design
    - Bottom up design is great for prototyping, so I start there if I don't have enough information about something to know how to test drive it
    - However, once I am actually touching user facing code, I do not let the design of the prototype influence the design of the application
    - All is test driven, I only use the prototype for learning
- Agile Design
    - Unplanned design leads to spaghetti code
    - Planned design leads to a bunch of stuff you don't need
    - Agile design allows the design to evolve as needed
- Test Driven Design
    - Tests are good at influencing the design towards design by contract
    - Tests are good at catching regressions after something has been confirmed to work
    - Tests are not a substitute for actually checking if the thing works initially
    - [types of tests](http://seanshubin.com/types-of-tests.svg)
- Design by Contract
    - I organize code in logical units such that
        - a contract is implied by naming, signature, interface, specification, or documentation.
        - the caller is responsible for the preconditions of the contract
        - the implementor is responsible for the postconditions and invariants of the contract
        - an exception is thrown if and only if the contract cannot be fulfilled
        - this is the opposite of defensive programming
- Service Oriented Architecture
    - The only shared design is at the network specification (in this case, http)
    - Other than that, application and persistence know nothing about each other
    - There is no shared code, no binary dependency relationship, and they are never loaded into the same jvm
    - This frees up both application and persistence to be as simple as they can be on their own, at the cost of some duplication
    - Every logical data store should be behind its own service
- Layered Architecture
    - from Domain Driven Design
        - User Interface
        - Application
        - Domain
        - Infrastructure
    - todo-application (User Interface and Application)
    - todo-persistence (Domain and Infrastructure)
    - todo-specification (communication between Application and Domain)
- Lambda Architecture
    - at the persistence level
        - no updates
        - no deletes
    - this does not mean you can't have mutable persistence, but only the immutable persistence is canonical
    - mutable persistence must be entirely derivable from the immutable persistence
    - the essence of lambda architecture lies in how you organize your data
        - canonical data should be simple and permanent (remember: canonical, simple, permanent)
        - only allow data to be complex if it can be treated as transient, re-derived if necessary (remember: derived, complex, transient)
- No mocks
    - I was not sure if mocks were necessary, so I tried coding without them to see what particular pain leads me towards mocks
    - Turns out, I never needed them once I got better at stubs and fakes
    - First, some definitions from [Martin Fowler's Article](http://martinfowler.com/articles/mocksArentStubs.html)
        - Dummy objects are passed around but never actually used. Usually they are just used to fill parameter lists.
        - Fake objects actually have working implementations, but usually take some shortcut which makes them not suitable for production (an in memory database is a good example).
        - Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test. Stubs may also record information about calls, such as an email gateway stub that remembers the messages it 'sent', or maybe only how many messages it 'sent'.
        - Mocks are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.
    - I find that mocks encourage exercising rather than testing your code
    - sometimes exercising your code is what you want, such as for controllers that do nothing but delegate to other classes
    - but you can still test that with stubs or fakes, and with stubs you can refactor duplication
    - refactor enough duplication, and your stub starts to look more and more like a fake
    - and can re-use the behavior across many tests
- Given, when, then
    - no given a, when b, then c, when d, then e
    - if I need that, I will break it up into two tests
        - given a, when b, then c
        - given a b, when d, then e

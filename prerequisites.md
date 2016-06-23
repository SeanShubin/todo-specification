# Prerequisites to scala training sample project

### Make sure you can run Scala from the console
- when you type
    -     scala
- you should see something that starts with
    -     Welcome to Scala 2.11.8
- or whatever the latest version of Scala happens to be

### Check out the sample projects

    mkdir training
    cd training
    git clone https://github.com/SeanShubin/todo-application
    git clone https://github.com/SeanShubin/todo-persistence
    git clone https://github.com/SeanShubin/todo-specification

### Install specification to your local maven repository
    cd todo-specification
    mvn install

### If you are using an Integrated Development Environment
- Make sure it is set up so you can quickly tell if tests are green
    - In your todo-persistence project, run core/src/test/scala/com/seanshubin/todo/persistence/core/JettyRunnerTest
    - In your todo-application project, run core/src/test/scala/com/seanshubin/todo/application/core/JettyRunnerTest

## Run the sample projects

### Package and run persistence
    cd todo-persistence
    mvn package
    ./run.sh

### Package and run application
    cd todo-application
    mvn package
    ./run.sh

### Navigate to application
- [http://localhost:7001](http://localhost:7001)

# Prerequisites to scala training sample project

### Check out the sample projects

    mkdir training
    cd training
    git clone https://github.com/SeanShubin/todo-application
    git clone https://github.com/SeanShubin/todo-persistence
    git clone https://github.com/SeanShubin/todo-specification

### Install specification to your local maven repository
    cd todo-specification
    mvn install

### Set up your environment
- Make sure you can quickly tell if the following tests are green
    - todo-persistence/core/src/test/scala/com/seanshubin/todo/persistence/core/JettyRunnerTest.scala
    - todo-application/core/src/test/scala/com/seanshubin/todo/application/core/JettyRunnerTest.scala

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
- [http://localhost:7001]

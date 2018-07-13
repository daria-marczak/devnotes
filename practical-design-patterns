# Design Patterns

Design pattern is a proven concept that is used to solve a problem. The solution to the problem is not obvious. It describes a relationship. 

## Types
+ Creational – creation of new instances to an object
-- Constructor
-- Module – grouping of like methods
-- Factory
-- Singleton
+ Structural – make up of objects themselves
-- Decorator
-- Façade
-- Flyweight
+ Behavioral – how they operate and are related to each other

### Creational design patterns
Used to  create new objects and adapt it to the solution.

**Constructor** is a new object with its own object scope. The *new* keyword creates a constructor function. It links it to the object prototype and binds ``this`` to the new object scope. It also returns ``this``.

```
function ObjectName(param1, param2) {
    this.param1 = param1;
    this.param2 = param2;
    this.toString = function() {
        return this.param1 + this.param2;
    }
}
```

The ``toString()`` is actually a method.

### Module pattern
It's a simple way to group similar methods. Module pattern creates a "toolbox" of functions to use.

```
const Module = {
    method: function() {};
    nextMethod: function() {};
}
```

### Factory pattern
Simplifies object creation. Creates different objects based on need. Creates repository.

Instead of using ``const sth = require("");`` we can create a factory:

```
const repoFactory = function(repoType) {
    if (repoType === "task") {
        let taskRepo = require("…");
        return taskRepo
    }
    if (repoType === "user") {
        let repoType = require("…");
        return repoType;
    }
};

const task1 = new Task(repoFactory.getRepo("task")).get(1);
```

### Singleton
Used to restrict an object to one instance of that object across the application. Singleton is an entity that rembers the last time you used it before and hands back the same instance.

```
const TaskRepo = (function() {
    let taskRepo;
    function createRepo() {
        taskRepo = new Object("Task");
        return taskRepo
    }
    return {
        getInstance() {
            if(!taskRepo) {
                taskRepo = createRepo();
            }
            return taskRepo;
        }
    };
})();

const repo1 = TaskRepo.getInstance();
const repo2 = TaskRepo.getInstance();
```

Node.js uses the CommonJS module pattern. 

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

## Structural Design Patterns
Concerned with how objects are made up and simplify relationship between objects. They either extend functionality or simplify functionality.

### Decorator pattern
Is used to add new functionality to an existing object, without being obtrusive. It wraps an objects. It protects existing objects. Allows extended functionality.

```
    const Task = function(name) {
        this.name = name;
        this.completed = false;
    }
    
    Task.prototype.complete = function() {
        console.log("Completing task: " + this.name);
        this.completed = true;
    }
    
    Task.prototype.save = function() {
        console.log("Saving task: " + this.name);
    }
    
    const urgentTask = new Task("Urgent Task");
    urgentTask.priority = 2;
    urgentTask.notify = function() {
        console.log("notifying");
    }
    
    urgentTask.notify = function() {
        console.log("Notifying");
    }
    
    urgentTask.save = function() {
        this.notify();
        Task.prototype.save.call(this);
    }
```
We have decorated urgentTask with priority and a new method. We can also decorate original methods and this does not break the original Task object.

```
const Task = function(name) {
    this.name = name;
    this.completed = false;
}

Task.prototype.complete = function() {
    console.log("Completing task: " + this.name);
    this.completed = true;
}

Task.prototype.save = function() {
    console.log("Saving task: " + this.name);
}

const UrgentTask = function(name, prority) {
    Task.call(this, name);
    this.priority = priority;
}
```
We are actually wrapping the Task in UrgentTask to have an UrgentTask with 3 others properties. But we do not have the prototypes, the mthods.

```
UrgentTask.prototype = Object.create(Task.prototype);
```
This will make a new object for the prototype out of task prototype. 

Now we can do this and the Task is still the same.

```
urgentTask.prototype.save = function() {
    console.log("Saving task: " + this.name);
    Task.prototype.save.call(this);
}
```

### Façade pattern
Used to provide a simplified interface to a complicated system. It hides the chaos from us. All we have is a simplified interface. JQuery is probably the most well-known façade pattern implemented. 

```
const Task = function(data) {
    this.name = data.name;
    this.priority = data.priority;
    this.project = data.project;
    this.user = data.user;
    this.completed = data.completed;
};

const TaskService = function() {
    return {
        complete: function(task) {
            task.completed = true;
            console.log("Completing task" + task.name);
        },
        setCompleteDate: function(task) {
            task.completedDate = new Date();
            console.log(task.name + " completed on " + task.completedDate);
        },
        notifyCompletion: function(task, user) {
            console.log("Notyfing " + user + " of the completion of " + task.name);
        },
        save: function(task) {
            console.log("Saving task " + task.name);
        }
    }
}();

const myTask = new Task({
    name: "MyTask",
    priority: 1,
    project: "Courses",
    user: "Daria",
    completed: false
});

TaskService.complete(myTask);
if (myTask.completed == true) {
    TaskService.setCompleteDate(myTask);
    TaskService.notifyCompletion(myTask, myTask.user);
    TaskService.save(myTask);
}
```
We can then create a façade on the top of it. We are not adding functionality with a façade pattern. We are only covering it and creating and better interface. 

```
    const TaskServiceWrapper = function() {
        const completeAndNotify = function(task) {
            TaskService.complete(myTask);
            if (myTask.completed == true) {
                TaskService.setCompleteDate(myTask);
                TaskService.notifyCompletion(myTask, myTask.user);
                TaskService.save(myTask);
            }
        };
        return { completeAndNotify };
    }();
    
    TaskServiceWrapper.completeAndNotify(myTask);
```

### Flyweight pattern
Conserves memory by sharing portions of an objecct between objects. This is only useful if we have a large number of objects.

```
    const Task = function(data) {
        this.name = data.name;
        this.priority = data.priority;
        this.project = data.project;
        this.user = data.user;
        this.completed = data.completed;
    };
    
    const FlyweightFactory = function() {
        
    }();

    function TaskCollection() {
        let tasks = {};
        let count = 0;
        const add = function(data) {
            tasks[data.name] = new Task(data);
            count++;
        };
        const get = function(name) {
            return tasks[name];
        };
        const getCount = function() {
            return count;
        };
        return {
            add, get, getCount
        }
    }
    
    const tasks = new TaskCollection();
    
    const projects = ["none", "courses", "training", "project"];
    const priorities = [1, 2, 3, 4, 5];
    const users = ["Jon", "Erica", "Amanda", "Nathan"];
    const completed = [true, false];
```

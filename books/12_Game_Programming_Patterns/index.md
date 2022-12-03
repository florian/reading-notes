# Game Programming Patterns [[website]](https://gameprogrammingpatterns.com/)

## 2) Command

- `Command`s are an object-oriented replacement for callbacks. They represent a function call
- For games these are nice because we can have a component which generates a `Command` from keypresses, and then call this Command with some local context
- The real power of commands show when building an undo/redo functionality. Here, the ``Command``  is an interface with two functions: `execute` and `undo`
- We keep track of all Commands exercised and an index to the current one. When users want to undo, we call its ``undo`` method and move the index to the previous `Command`
- This list of ``Command``s  also allows for a replay of the game afterwards
- The ``Sandbox``, ``Flyweight``,  and Chain of Responsibility patterns are all variants of ``Command``. We will learn more about them in the rest of the book

## 3) Flyweight

- Imagine your game wants to render a huge grid of tiles
- You could represent each tile using an object. However, that would be wasteful because there are a lot of them and for the most part they share most state
- Flyweight is a pattern for minimizing memory cost by sharing state. Here, tiles would reference a ``ModelTile`` that contains the shared information
- Flyweights store context-dependent state themselves and refer for context-independent state to the model object
- When instances are immutable, you can share them instead of creating multiple of them
- These ideas are also useful for ``Command``s

## 4) Observer

- Observers are a way of decoupling components
- Components can publish events and subscribe to them. By communicating via these events, the publisher does not need to know about subscribers, and these components are thus decoupled
- Imagine an achievement system in a game. The achievement management can be decoupled from the game using the ``Observer``  pattern. Game components publish various events for what is hapening and the achievement management subscribes to them
- Another application: The physics engine publishes events about what is happening and the game then just subscribes to these to update the game state. Similarly the audio engine could subscribe to play various sounds
- Instead of maintaining an array of subscribers, one can also use a linked list, which is more optimal when commonly adding and removing elements
- > The reason design patterns get a bad rap is because people apply good patterns to the wrong problem and end up making things worse.
- There can be memory leak problems with ``Observer``s when not properly cleaning up subscriptions after objects go out of memory. Even in languages with garbage collection, this can happen. This is called the [lapsed listener problem]([https://en.wikipedia.org/wiki/Lapsed_listener_problem](https://en.wikipedia.org/wiki/Lapsed_listener_problem))
- Binding via `Observer`s happens at runtime. Sometimes you want static compile checks. In those cases, an explicit coupling can be a better choice

## 5) Prototype

- Imagine a function for spawning new enemies with their own state. It is called quite often and needs to be fast
- Instead of creating new objects from scratch in it, ``Prototype`` offers an alternative: For each kind of enemy you instantiate a ``Prototype`` instance. When the spawn function is called, it just copies the ``Prototype`` instance and returns it
- This _can_ be much faster to do than to build an instance from scratch
- The author is not convinced that this is really the case often. Instead, he suggests that a more interesting application are prototypes as a language paradigm
- Prototypes are an alternative to classes. Instead of distinguishing between a class and an instance, prototypes mix these. Any object has a prototype associated with that you can use to spawn new objects
- JavaScript is the most popular prototype-based language, but it exposes prototypes in a way that actually kind of look like classes. The language Self offers prototypes in a purer way

## 6) Singleton

- A singleton ensures that a class only has a single instance and makes that instance globally accessible
- Note how the singleton does two things in a single pattern
- It is usually needed when interacting with some outside system that has its own state. For example, this is the case for a file system that receives async requests. By using a singleton for the connection to the file system, you can ensure that locks are used in the right places
- The major disadvantage is that singletons are like global variables, which should be avoided for many good reasons
- A lot of times the component does not really need to be a singleton and there is a better way of setting it up
- The author never used singletons when working on games

## 7) State

- Managing all possible actions and states in your game can be a lot of detailed work with a lot of if conditions. It is easy to forget to implement behavior for one of these, e.g. when a character wants to jump but is currently diving under water
- By instead using a state machine you can be more explicit about possible states and transitions
- The state machine is neatly encapsulated in its own class which other objects then refer to
- Hierarchical state machines are subclassed state machines that partly refer to the parent one
- Pushdown automata allow using a stack of states, not just a single enum. This makes them much more powerful because they can preserve historical information without a combinatorial explosion of states

## 8) Double Buffer

- Computers will periodically render whatever was written into the framebuffer
- When you write to the framebuffer too slowly, that might lead to just part of something being rendered in a given frame
- The solution is the double buffer: You maintain two buffers. One is the one currently used as the framebuffer. The other is for accumulating changes. When you want to display the changes, you swap the pointers to the buffers
- More generally, this is a useful pattern when state is modified incrementally, the state might be accessed during modification and we want to avoid others from reading the work in progress
- This not just useful for graphics but for other things, e.g. when a bunch of objects manipulate each other's state in turns

## 9) Game Loop

- *Batch mode* programs: Input is once provided, program is executed, and then returns output
- *Event loop*: An infinite loop that checks whether any user input was provided and then reacts to it. If no input was provided, it does not wait but the next iteration of the loop continues. A word processor for example is based on such a loop
- How many loop iterations are executed per seconds is called *frames per seconds* (*fps*). This is determined by what work is performed per iteration and by the capability of the hardware that executes this work
- In the old days, hardware would dictate the maximum fps
- A *game loop* is just an event loop specialized for games. It typically does something like this in each iteration:
    - Process user input
    - Update the game state
    - Render the game state
    - Track how much time passed and determine whether to `sleep` before the next iteration
- The update step can be parameterized by how much time passed, meaning it can make bigger updates when more time passed. This results in normalizing how quickly the game state changes irrespective of how fast the local hardware is
- An alternative to parameterizing the update function is to call it multiple times in one game loop iteration while staying above a certain fps requirement. This can prevent some nasty rounding errors that could appear when multiplying numbers in the update function with a time spent parameter
- In some environments you can call into the system’s event loop and do not have to build your own one. This is e.g. the case with JavaScript and browsers. If you choose to do this, you give up flexibility in return for better integration with the rest of the work the system is doing
- On desktop computers you want to use as many fps as possible. On mobile phones however, you don't want to unnecessarily drain the battery, so a common practice is to limit the maximum fps and sleep in the remaining time

## 10) Update

- The game loop takes care of updating state each frame
- Scattering all the code for updating in the loop is not maintainable. Instead, one can maintain a list of entities that expose an `update` method. The game loop then iterates over these objects and calls their `update` method
- The entity objects maintain state between update calls
- One has to be careful about manipulating the list of entities during the update process, e.g. to add or remove objects. There are standard solutions for all of these manipulations though, e.g. maintaining a separate list of elements to remove or iterating through the list in backwards order (which ensures that one does not touch outdated indices)

## 11) Bytecode

- Making it possible to create mods for games is useful because it means that third party developers can enhance the game. It also means you can ship some changes without changes to the binary
- Custom bytecode with a custom virtual machine (VM) is a solution to this
- Developers can write this bytecode (e.g. using a compiler or GUI you develop) and have it execute in your VM
- This allows you to expose certain functionality while still running the code in a sandboxed environment. It also means that outside developers don't have to have their code compiled into your binary
- Developing a bytecode format and a VM comes with all the standard challenges
    - How do you access relevant values? By keeping them on the stack or by allowing bytecode to specify registers?
    - Are there multiple types? If so, how are they represented? Via tags, untagged unions, objects, or are they only distinguished during compile time?

## 12) Subclass Sandbox

- In some games you might have lot of very similar classes, e.g. 100 different types of superheroes
- Of course you want to avoid reimplementing the same behavior over and over
- One possible solution is a subclass sandbox:
    - You implement a base class with protected methods for all shared functionality
    - Different types then subclass this and add their custom behavior
- There are different levels to how close to a sandbox this really is: In the extreme case, the subclasses don't access anything from the outside other than the protected methods
- This leads to a wide but shallow inheritance tree
- (*Note: This is probably the pattern with the weakest sell in this book. It seems like there are a bunch of potentially preferable ways of designing this*)

## 13) Type

- Writing code for 100 different classes (e.g. superheroes or monsters) that are nearly the same is quite cumbersome
- Instead, it can be preferable to have a config file that configures the individual class properties. This config file can then be used to automatically create the classes
- You only implement two classes from hand, e.g. `Monster` and `Breed`
- The `Monster` instances are created using the config file. These represent factories for the actual instances
- The `Breed` instances are the actual instances that hold mutable state. They are created by taking values from their respective `Monster` type

## 14) Component

- A class for some entity in the game shouldn't need to handle state, graphics, physics and sound all by itself. Instead, it's preferable to have well-encapsulated components for each of these
- With this design pattern, one would define a state `Component`, a graphics `Component`, a physics `Component` and a sound `Component` for each entity
- These are all bundled by one generic class that calls `update` methods on all of them
- Different components either talk directly with each other or through an event system

## 15) Event Queue

- In the event systems described earlier in the book, observers directly react to messages they receive
- This does not play nicely with game loops, which keep track of the time passed in the game
- An alternative that plays nicely with a game loop is an *event queue*. This decouples when a message is received from when it was sent
- All events are stored in a global event queue. When the game loop ticks, events are read from the queue
- The queue might have some logic for efficiently depatching events to the right components, [e.g](http://e.gm). instead of all mousemove events, it might forward those happening in a specific viewport relevant to the subscriber
- The queue can be implemented using a ring buffer

## 16) Service Locator

- There are some components that get used all over the code base, e.g. a logging system or audio playing functionality
- Rather than directly using a specific such service, you can use the service locator pattern: There's a global entry point for requesting and providing a certain service
- This decouples providing and using the service. You can flexibly swap out the provider without touching other parts of the code base
- A dummy provider can be used to gracefully handle the case that the provider is not yet available. This can also be useful during debugging
- This pattern is a bit like using a dependency injection framework except that the providing part is dynamic and could be anywhere in the code base, which is both more flexible but also makes it even harder to reason about what is used

## 17) Data Locality

- Over time, compute has gotten much faster while accessing data barely has
- When data is fetched from memory, usually contiguous 64 to 128 bytes are retrieved and cached. If the cached values are then requested afterwards, they can be read very efficiently. If something not cached is requested, we talk of a *cache miss*
- That means that it pays off to keep data that you contiguously access in contiguous memory, e.g. in neighboring fields of an array
- The design pattern then really just is to organize data in performance-critical paths that way. This works especially well when the previously introduced Component design pattern is used

## 18) Dirty Flag

- Sometimes you need to recursively recompute something if a certain value changed. Not all the values in the recursion tree need to be recomputed though
- You can use flags (set bits) for the values that need recomputing. The values that do not need recomputing are read from the cache
- This is not just useful for saving communication but also for saving unnecessary network traffic
- Some web frameworks like Angular use this pattern to figure out which data has changed

## 19) Object Pool

- There are some objects that you will need a lot of instances of, for example particles in an animation
- Keeping these all on the heap where they are dynamically allocated and freed can be wasteful because it could lead to the free memory being fragmented. That's bad for data locality, as explained in chapter
- The object pool pattern is meant to solve this issue. It manages a large amount of instances. These instances are kept in one long array and are thus close in memory.
- When instances get destroyed or created, the pool makes sure that all active instances are next to each other in memory
- The pool supports calling a method on all active instances, [e.g](http://e.gm). to animate them

## 20) Spatial Partition

- In many games, objects exist in a physical world and have a location. A lot of times you need to find objects that are in some distance to each other
- To make that more efficient, you can use a data structure optimized for such a task
- A simple one is dividing the world into a grid of cells. Each object is placed into one cell depending on its position. To find close elements you then only compare an object with others in the same cell and in neighboring ones
- This can work well in some applications but is inefficient if most of the cells are empty because all objects are in one corner of the world
- *Quadtrees* are an improvement over this: If there are fewer than *K* elements, they are kept in a flat data structure. If there are more they are divided into a grid of four cells. The procedure is repeated recursively for each of the four cells
- *Octrees* are the same thing in 3D
- More generally speaking, such data structures can be analysed in a couple of characteristics:
    - Flat vs hierarchical partitioning: Are the elements divided once or recursively
    - Is partitioning object-dependent or -independent: If it's dependent, then adding elements once can be more efficient, but moving elements becomes complicated
- Additionally to using such a data structure you might want to keep the same elements in a flat data structure to be able to iterate over them quickly

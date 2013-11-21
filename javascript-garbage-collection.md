# Javascript Garbage Collection
#### You can't control when Javascript does garbage collection, but you can control how your application memory is managed

Colt McAnlis gave [an enthusiastic and animated presentation](http://www.youtube.com/watch?v=Op52liUjvSk) at Velocity NYC 2013 about memory management in Javascript. He started out by acknowledging that most Javsscript developers actively resist the subject, preferring to respect the automated memory management that Javascript performs in the background, although few understand how it really works.

## Garbage Collection and Frame Rate
The smoothness of your application's performance is reflected in the framerate. Even if you tune your application so all normal behavior is within 16.7 milliseconds, you won't see smooth 60 frames-per-second performance because every now and then Javascript will run garbage collection to reclaim freed memory from objects that are no longer in scope.

Intentionally managing memory algorithms optimizes the way Javascript does garbage collection. You need six times the memory requirements for your application if you want garbage collection to run smoothly without stuttering.

## When Does Garbage Collection Occur?
Garbage collection happens more often when more objects are being created more frequently. The algorithm varies from browser to browser, but the concept is consistent. As long as Javascript sees new objects being created and destroyed, it will issue intermittent garbage collection routines that will slow down performance.

Chrome dev tools timeline view and the Adobe GC Tool can help expose garbage collection cycles, and track the amount of memory you need to pre-allocate in order to avoid garbage collection cycles.

## Static Memory Javascript
One way to circumvent this behavior is to create a pool of objects at the start of your app, and use it as a source. Whenever you need a new object, you grab one from the pool. When you're done with it, you nullify it an add it back to the pool. You have to null out any references before sending your null object back to the pool, otherwise stale references will lead to memory leaks.

One example of how this works in practice comes from [Emscripten](https://github.com/kripken/emscripten), which converts C to Javascript. But Javascript generated from C code in Emscripten runs faster than comparabe functions written in traditional Javascript. That's because Emscripten ehnances performacy by pre-allocating a large memory heap, and then managing all objects in memory to use that heap. Emscripten was used to create [Asm.js](http://asmjs.org/), which is a subset of Javascript optimized to use static memory space that isn't subject to garbage collection.

It is possible for any Javascript developer to do the same thing. It may not be simple, but it is essential if you want to be in control of how your web application will perform. You can't control the browser or the memory availability of your visitors, but you can establish your requirements and enforce them to maintain consistent performance.

## How Big a Heap Do You Need?
Every time you exceed the boundaries of your heap, your old heap will be deleted and replaced by a new one that's twice the size, and garbage collection will be triggered. It's best to make the heap as large as you need to begin with.

Of course, if you make it too large, your app will require more memory than it needs to run. That can impact performance and also adoption, so be judicious and conservative in your estimates. Determining how large the heap needs to be involves a lot of testing and logging, to see how many objects your application needs to create in all typical use cases.

## Closures and Memory Leakage
The jQuery library and most social media widgets use a lot of closures, which maintain scope that leaks memory when the function that created the closure ends. If you need to use closures to get the job done, be sure you know how they are using memory. 

There is no free memory. Move toward static functions.


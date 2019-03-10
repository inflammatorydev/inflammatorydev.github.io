---
layout: default
---

When running a script, it is often hard to work out exactly when a particular function is going to fire relative to all the other functions running in the event loop. So let's start with a pop quiz.

```
const a = () => {
  return console.log('a');
}

const b = () => {
  const myPromise = Promise.resolve('b').then(res => console.log(res));
  return myPromise;
}

const c = () => {
  return setTimeout(()=>{
  console.log('c');
  },0);
}

const d = () => {
  return console.log('d');
}

a();
b();
c();
d();
```

**What will be logged out and in which order?**

So talking it through we call a() first, which logs out 'a'. Next we call b(). This is a promise, that is immediately resolved as 'b', so I guess we can say that 'b' will be next. c() does a setTimeout of zero seconds before logging out 'c', so I guess maybe its called straight away? Finally we have d(), a straight log of 'd'.

**So I guess its 'a', 'b', 'c' and then 'd'**

Well lets be honest, it would be a pretty straightforward article if that was the actual answer.

So what is it then?

**Its actually 'a', 'd', 'b', 'c'**

![](https://i.imgflip.com/2s4s48.jpg)

To understand why this is, we need to know 4 key things.

The Event Loop, Call Stack, the Queue and the Job Queue (also known as microtasks).

The Event Loop is a loop that just runs continously looking for stuff to do. At the end of each loop it will peek into the Job Queue to see if there is anything it needs to do from there before continuing on with the main loop.

The Call Stack is a list of all the functions to be executed within this particular event loop cycle.

The Queue is where the event loop puts a task to do at a later date because it is not ready to go onto the call stack. For instance a setTimeout will put a function onto the Queue ready to be called after 'n' milliseconds.

The Job Queue is where we put our resolved promises ready to be actioned. These can only be actioned after the call stack has been emptied of its task for that particular event loop.

![Chinese Salty Egg](./updated-event-loop1.png)

_Clear as mud?_ Great, so lets go!

We kick off the event loop ... and we are now running from the very top of the script. Firstly we hit the declarations of the functions a, b, c, d. Remember they are just declarations, so at this point we still have nothing on the callstack. Next we hit a call for the function 'a'.

```
a();
```

![Chinese Salty Egg](./updated-event-loop3.png)

OK, finally we are 'doing' something, so function 'a' goes onto the call stack and is executed, once complete it is removed from the call stack.

<div style="background-color: #fbb; padding: 20px; margin-bottom: 20px; border-radius: 5px;">Note for brevity I will show just the function going onto the call stack, rather than each action within the function as I dont want to focus on the trees and you miss out on seeing the forest.</div>

Next up we have a call for 'b'. Lets pop that onto the call stack.

'b' creates a promise that is immediately resolved. But we can't do anything with this straight away. Instead, we have to put the promise into the Jobs Queue. All promises go into this queue, to be processed once everything is finished on the call stack.

So currently 'a' and 'b' have been processed, now onto 'c'. This contains a setTimeout which contains a function to be fired after zero seconds. What this means in reality is that because its a setTimeout, we dont immediately fire it but instead put it into the 'Queue' (remember this is different from the Jobs Queue that currently contains function 'b').

Finally we fire function 'd'. 'd' goes onto the call stack. The console log is fired, 'd' is then removed from the call stack as it has now completed.

And thats it ... we are done. So how do things currently stand?

![Chinese Salty Egg](./updated-event-loop6.png)

The call stack is now empty. In the Queue we have function 'c' and in the Job Queue we have function 'd'.

Before we tick over to the next event loop, we need to check the Job Queue to see if anything else needs to be done. This contains one job, function 'b' which has resolved. So we log out 'b' to the console.

And now we are finished with this particular event loop. So lets go around again with the next event loop.

At this point hopefully some of you remember that we still haven't done anything with function 'c', currently sitting in the Queue. Now that we've started a new event loop ... lets check the queue to see if anything is ready to come out. Because function 'c' was within a setTimeout set to zero seconds, its ready to come onto the Call Stack, so on it goes.

Because there is nothing else to action, we execute function 'c' on the Call Stack and thats it.

So quick recap.

Functions 'a' and 'd' are synchronous functions and put onto the Call Stack in turn and immediately fired. Then at the end of the event loop, BEFORE we tick over to the next event loop, we check the Jobs Queue for any resolved promises. We have one, function 'b', so we execute that, which means now we have logged out 'a', 'd' and 'b' ... then with one function left that can only be added during the next event loop, we do that and log out 'b' <br />... _giving us 'a', 'd', 'b' and 'c'_.

_yay_

[back](./)

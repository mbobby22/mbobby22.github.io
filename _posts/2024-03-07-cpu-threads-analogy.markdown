---
layout: post
title:  "CPU Threads"
date:   2024-03-07 20:00:00 +0200
categories: coding architecture
---

I found a very good analogy to explain what **CPU threads** are and how they work and I will be sharing this with you.



{% highlight quote %}

I run a hotel, where guests are switched between rooms.
I can host hundreds of guests in parallel. 
One guest leaves a room, another enters it.
If the rooms are full, guests wait in the lobby, but I quickly switch them, so every guest gets time in a room.
I don't get then why I have a room limit on my hotel building.
I mean I fastly switch guests between rooms then why does my building have 8 rooms/16 beds?

hotel = CPU
room = CPU core
bed = CPU thread
guest = software thread

{% endhighlight %}

Source: [CPU threads analogy][cpu-threads-analogy]{:target="_blank" rel="noopener"}

[cpu-threads-analogy]: https://stackoverflow.com/a/50039686 

This gets us to the question: **What is the point of more threads than cores?**

If you have work that needs to be run, one common mechanism is to use a `threadpool`.
It might seem to make sense to have the same number of threads as cores, yet the [.Net threadpool has up to 250 threads available][dotnet]{:target="_blank" rel="noopener"} per processor.
I'm not certain why they do this, but my guess is to do with the size of the tasks that are given to run on the threads.

This will get us to **Go routines** topic, please check the [article here][blog-link].

Source: [Multithreading topic][multithreading]{:target="_blank" rel="noopener"} 

[multithreading]: https://stackoverflow.com/a/50039686
[dotnet]: http://msdn.microsoft.com/en-us/library/system.threading.threadpool.aspx
[blog-link]: http://mbobby22.github.io
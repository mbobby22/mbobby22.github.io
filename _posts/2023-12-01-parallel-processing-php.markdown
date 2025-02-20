---
layout: post
title:  "Parallel processing with PHP"
date:   2023-12-01 08:00:00 +0200
categories: coding architecture
---

After I became a **Go developer** my first initial thoughts were:
do I really need this or is `Golang` providing just another way to accomplish the same things that can be done with `PHP` already?

So I started digging out stuff online just to make sure I haven't missed any PHP features that I was not aware of.

I found some useful resources and included the links here.

[Official docs][php-parallel-docs]{:target="_blank" rel="noopener"} say:

{% highlight quote %}
This philosophy which is embraced by parallel has its origins in Go,
one of the most widely admired if not used platforms for writing parallel code at the moment.
Go programmers have to work hard to live up to this ideal:
PHP and parallel do all the hard work for the programmer, and by default.
{% endhighlight %}

Why would you need [parallelism in your project][php-processing-parallel]{:target="_blank" rel="noopener"}?

{% highlight quote %}
Parallel processing in PHP refers to the ability of executing multiple tasks simultaneously,
instead of sequentially, on a single processor or multiple processors.
In simple terms, it means dividing a larger task into smaller,
independent tasks that can be executed concurrently to increase the overall speed and efficiency of the program.
{% endhighlight %}

So far I tested out [Pthreads extension][pthreads-ext]{:target="_blank" rel="noopener"} and with the help of [krakjoe][krakjoe-pthreads]{:target="_blank" rel="noopener"} I made a POC which can be found in my **Bitbucket** [php threads demo repo][php-threads-repo]{:target="_blank" rel="follow"}.

{% highlight php %}
<?php
class MyThread extends Thread
{
    public function run()
    {
        echo "Hello from thread " . $this->getThreadId() . "\n";
    }
}

$thread1 = new MyThread();
$thread2 = new MyThread();

$thread1->start();
$thread2->start();

$thread1->join();
$thread2->join();
{% endhighlight %}

Check out [this link][php-processing-parallel]{:target="_blank" rel="noopener"} to find everything you need to get it done.

Also this [How to run PHP code in parallel][php-parallel]{:target="_blank" rel="noopener"} (the easy way) might help.

<h3>Note***</h3>
What you need to note here is that we need a special [PHP zts build][php-zts-build]{:target="_blank" rel="noopener"} for this, it will not work with common `cli` or `fpm` builds.
Also, you need to make sure the [pthreads extension][php-threads-build]{:target="_blank" rel="noopener"} is enabled.


[php-parallel-docs]: https://www.php.net/manual/en/philosophy.parallel.php
[php-processing-parallel]: https://www.atatus.com/blog/parallel-processing-in-php
[php-parallel]: https://stitcher.io/blog/parallel-php
[krakjoe-pthreads]: https://github.com/mbobby22/pthreads
[pthreads-ext]: https://www.php.net/manual/en/book.pthreads.php
[php-zts-build]: https://stackoverflow.com/questions/39829944/php-enable-zts-pthreads
[php-threads-build]: https://stackoverflow.com/questions/34969325/how-to-install-php7-zts-pthreads-on-ubuntu-14-04?rq=4
[php-threads-repo]: https://bitbucket.org/mbobby22/php-threads-demo/src/main/
---
layout: post
title:  "PHP nested Exceptions setup"
date:   2018-04-02 19:55:00 +0200
categories: coding php exceptions
---

{% highlight php %}
<?php

class DbException extends Exception
{}

class Db
{
    public function query()
    {
        throw new DbException('Could not connect to database.');
    }
}

class FooException extends Exception
{}

class Foo
{
    public function doFoo()
    {
        try {
            $db = new Db();
            $db->query();
            //throw new Exception('xyz');
        } catch (DbException $e) {
            throw new BarException('Database issue ', null, $e);
        } catch (Exception $e) {
            throw new FooException('Error in foo ', null, $e);
        }
    }
}

class BarException extends Exception
{}

class Bar
{
    public function doBar()
    {
        try {
            $foo = new Foo();
            $foo->doFoo();
        } catch (Exception $e) {
            throw new BarException('Error in bar ', null, $e);
        }
    }
}

try {
    $bar = new Bar();
    $bar->doBar();
} catch (Exception $e) {
    echo $e->getMessage();
    if ($e->getPrevious()) {
        echo '<br>';
        echo $e->getPrevious()->getMessage();
        echo '<br>';
        echo $e->getPrevious()->getPrevious()->getMessage();
    }
}

die();
{% endhighlight %}

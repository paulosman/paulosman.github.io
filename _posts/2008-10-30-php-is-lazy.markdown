--- 
layout: post
title: PHP is Lazy
wordpress_id: 36
wordpress_url: http://blog.paulosman.com/?p=36
date: 2008-10-30 20:10:54 -04:00
---

Last week I started reading [Extending and Embedding PHP](http://www.amazon.ca/Extending-Embedding-PHP-Sara-Golemon/dp/067232704X/ref=sr_1_1?ie=UTF8&s=books&qid=1225346121&sr=8-1). I really like extending scripting languages for a number of reasons, most of all because it's a great way to learn about the more esoteric features of a language. So far I've discovered two neat things about PHP that made me smile, so I thought I'd share them here.


<h3>Copy on Write</h3>
<p>In an effort to save CPU cycles, PHP uses an optimization strategy called "Copy on Write". Basically what this means is that variable data isn't copied until it needs to be. Using an example similar to the one found in the book:</p>
<pre lang="php">
$a = 50;
$b = $a;
$b += 5;
</pre>
<p>
At the first line, the variable $a is created and memory is allocated for it. When we set $b to equal $a however, you might think that the data in $a is copied to $b. PHP doesn't actually copy the data until the 3rd line when $b is incremented by 5. The value in this is that data is not copied unnecessarily. If, for some reason, we took out the 3rd line and $b was never modified after the initial assignment, the data would actually never have to be copied.  
</p>

<h3>Return Value Used?</h3>

<p>The second, admittedly more surprising thing I learned was that PHP provides internal functions a parameter called <code>return_value_used</code> whose value can be inspected to determine whether the return value being prepared by the function is actually being saved or not. Imagine you have a function that does some heavy crunching to compute a return value, and a caller (for some reason beyond us) does this:</p>

<pre lang="php" escaped="true">
&lt;?php
...
myextension_compute_something();
...
?&gt;
</pre>

<p>In other words, they call the function but don't store the return value. Well, being a thoughtful internal function author, you can check to see that <code>return_value_used</code> evaluates to false and just skip doing any work, saving CPU cycles and time, like so:
</p>

<pre lang="c">
PHP_FUNCTION(myextension_compute_something)
{
    if (return_value_used) {
        // some fancy but expensive algorithm
        // ...
        RETURN_DOUBLE(some_value);
    } else {
        RETURN_NULL();
    }
}
</pre>

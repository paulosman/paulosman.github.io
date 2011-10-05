--- 
layout: post
title: Bad Signs - PHP's "Shut Up" (@) Operator
wordpress_id: 40
wordpress_url: http://blog.paulosman.com/?p=40
date: 2009-01-04 03:22:21 -05:00
---
<p><a href="http://derickrethans.nl">Derick Rethans</a> has a <a href="http://derickrethans.nl/five_reasons_why_the_shutop_operator_@_should_be_avoided.php">post</a> about PHP's "shut-up" operator (@) and why it should be avoided. He outlines a fairly common debugging scenario and gives some "under-the-hood" explanations on why that particular operator sucks. I couldn't agree more (that it should be avoided) and I want to go further and talk about something that has always bugged me about that operator. In my opinion, it's a hackish, band-aid fix to a much larger, much more worrisome problem: horrendous API design.</p>

<p>First, a disclaimer. I know that there are some historical reasons for a lot of these things (i.e. Exceptions not appearing until PHP5) but that doesn't change the current reality. Consider for instance filesystem functions in the standard extension:</p>

<pre lang="php">
$lines = file("/tmp/file.txt");
</pre>

If the file "/tmp/file.txt" does not exist, depending on your php.ini settings (because display_errors should always be 'Off' on production systems), you may get something similar to the following output:

    Warning: file(/tmp/file.txt) [function.file]: failed to open stream: No such file or directory in /path/to/your/docroot/test.php on line 3


<p>Now the proper thing to do of course would be to verify up front that the file exists and is readable with a call to <a href="http://php.net/is_readable">is_readable()</a> or something similar, but imagine this is a case where there is no way to determine ahead of time if a warning or error will be generated (Derick mentions silencing network errors with <a href="http://php.net/stream_socket_client">stream_socket_client()</a> as one example). In this case, you might be tempted to use the shut-up operator and depend on the return value as the one true way to see if something was succesful or not.</p>

<p>Now compare this with what languages like Python (for example) do, If you were to write similar code in Python:</p>

<pre lang="python">
fp = file("/tmp/file.txt")
</pre>


If the file did not exist, you'll get an IOError which, if left uncaught, will result in the following being written to standard error:

    Traceback (most recent call last):
    File "test.py", line 1, in <module>
    IOError: [Errno 2] No such file or directory: '/tmp/test.txt'

<p>The difference here of course is that an IOError can be caught, ignored or whatever best suits your needs. It's a part of the language, the API is not just spewing out garbage to standard output or standard error. More importantly, having a function (or technically a constructor) like file() throw an exception follows a consistent design philosophy whereas PHP's error system always leaves me guessing or consulting the docs.</p>

<p>I guess what I'm saying is that I think the shut-up operator is just one of those signs that PHP is suffering from some serious cruft and I don't think those kinds of things bode well for a language.</p>

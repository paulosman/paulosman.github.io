---
layout: post
title: Building Services with Apache Thrift
---

I've always been interested in cross-language service frameworks. I
believe in using the best tools for a job, instead of being limited to
a specific language or framework, so being able to write components of
a service in whatever langauge makes the most sense is attractive to
me. In past lives, I've developed software that used
[CORBA](http://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture),
[SOAP](http://en.wikipedia.org/wiki/SOAP),
[COM](http://en.wikipedia.org/wiki/Component_Object_Model) and
[XPCOM](https://developer.mozilla.org/en/XPCOM) and found that they
all suck in different, significant ways. Because of this, I've been
interested in the Apache Thrift project originally developed at
Facebook.

At its core, Thrift is an interface definition language, a code
generation tool and a set of libraries that take care of serialization
and implement the transport protocol.

In order to generate code from a thrift service definition, you'll
need to install the thrift compiler. You can install it with
[homebrew](http://mxcl.github.com/homebrew/) on a mac or download it
from the [project site](http://thrift.apache.org/download/).

Thrift services and datatypes are defined using an
[IDL](http://en.wikipedia.org/wiki/Interface_description_language)
that should look pretty familiar to anyone who has worked with CORBA
or similar technologies. Let's say you want to create a service that
handles creation and storage of user objects.  We'll write the service
in Python and a client in Ruby. This is obviously a contrived example,
but imagine that we're using some kind of data store that has a
library written in Python and all of our application code is in
Ruby. It'd be great to be able to use the data store from our Ruby
code without having to develop and maintain our own library.

We start by defining the datatypes and service interface:

{% highlight thrift %}
struct User {                                                                                                      
  1:string first_name,                                                                                             
  2:string last_name,                                                                                              
  3:string email                                                                                                   
}                                                                                                                  
                                                                                                                   
service UserService {                                                                                              
  bool add_user(1:User user),                                                                                      
  list<User> get_users()                                                                                           
}                                                                                                                  
{% endhighlight %}

Save the above code in a file called users.thrift, then generate
the Python code by running <code>thrift --gen py users.thrift</code>.
Because we'll be writing a client in Ruby, we'll also want to run
<code>thrift --gen rb users.thrift</code> to generate the Ruby code.

Next, write the server in Python. This requires a bit of boilerplate
and a service handler that gets passed to the Thrift processor. Notice
that thrift threw the generated code in a directory called 'gen-py'.
It's just a Python module, so you can move it wherever you'd like, but
in this example I'll just modify sys.path appropriately.

There are a variety of options for the server itself. For simplicity,
we'll just use <code>TServer.SimpleServer</code> which is a
straightforward single threaded server. There's also a multithreaded
server and a forking server.

{% highlight python %}
import os
import sys

sys.path.append(os.path.join(
    os.path.dirname(os.path.abspath(__file__)), 'gen-py'))

from thrift.transport import TSocket, TTransport
from thrift.protocol import TBinaryProtocol
from thrift.server import TServer

from userservice import UserService


class UserServiceHandler(object):

    def __init__(self):
        self.users = []

    def add_user(self, user):
        self.users.append(user)
        return True

    def get_users(self):
        return self.users


def create_server(host='localhost', port=9090):
    handler = UserServiceHandler()
    return TServer.TSimpleServer(
        UserService.Processor(handler),
        TSocket.TServerSocket(host=host, port=port),
        TTransport.TBufferedTransportFactory(),
        TBinaryProtocol.TBinaryProtocolFactory()
    )

if __name__ == '__main__':
    server = create_server()
    server.serve()
{% endhighlight %}

Finally we write the client in Ruby. Ignore the boilerplate client
code for now, and focus on the part where we create a user, add it
using the service, then get a list of users from the service. This
part looks like normal Ruby code which is part of what I really like
about Thrift. There are no special types to use, users as returned
from the get_users method is a simple Array and everything feels
really natural.

{% highlight ruby %}
$:.push('./gen-rb')

require 'rubygems'
require 'thrift'
require 'user_service'
require 'userservice_types'

def create_client(host='localhost', port=9090):
  socket = Thrift::Socket.new(host=host, port=port)
  transport = Thrift::BufferedTransport.new(socket)
  protocol = Thrift::BinaryProtocol.new(transport)
  client = UserService::Client.new(protocol)
  return transport, client
end

user = User.new()
user.first_name = 'Paul'
user.last_name = 'Osman'
user.email = email

transport, client = create_client()
transport.open()
client.add_user(user)

users = client.get_users()
users.each do |user|
    puts "#{user.first_name}, #{user.last_name}, #{user.email}"
end

transport.close()
{% endhighlight %}

I found a [few](http://jnb.ociweb.com/jnb/jnbJun2009.html)
[good](http://wiki.apache.org/hadoop/Hbase/ThriftApi)
[resources](http://diwakergupta.github.com/thrift-missing-guide/thrift.pdf)
[out
there](http://yannramin.com/2008/07/19/using-facebook-thrift-with-python-and-hbase/),
but most were either a bit outdated, or too generic or went into areas
that I wasn't immediately interested in. I just wanted the straightest
line to seeing what Thrift was all about, so I thought I'd write that
up here.

If you're curious about Thrift, I would encourage you to read this
[whitepaper](http://thrift.apache.org/static/thrift-20070401.pdf)
before diving any deeper. It provides a good overview of how Thrift
works and why it was designed the way it was. I'm glad to see that
Thrift is gaining some adoption, appearing in projects like [HBase](http://wiki.apache.org/hadoop/Hbase/ThriftApi), [Storm](https://github.com/nathanmarz/storm/wiki/Using-non-JVM-languages-with-Storm) and [FlockDB](https://github.com/twitter/flockdb).


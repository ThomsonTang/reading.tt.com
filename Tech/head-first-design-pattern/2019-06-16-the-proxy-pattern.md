# Proxy Pattern

The Proxy Pattern provides a surrogate or placeholder for another object to control access to it.
Use the proxy pattern to create a representative object that controls access to another object, which may be remote, expensive to create or in need of securing.

Here are a few ways proxies control access:
• A remote proxy control access to a remote object.
• A virtual proxy controls access to a resource that is expensive to create.
• A protection proxy controls access to a resource based on access rights.

FAQs:

1. Are the Remote Proxy and Virtual Proxy really ONE pattern?
   There are lots of variants of the Proxy Pattern in the real world; what they all have in common is that they intercept a method invocation that the client is making on the subject. This level of indirection allows us to do many things, including dispatching requests to a remote subject, providing a representative of for an expensive object as it is created, or providing some level of protection that can determine which clients should be calling which methods.
2. Does Proxy seems just like Decorator?
   Sometime Proxy and Decorator look very similar, but their purpose are different: a decorator adds behavior to a class and and never get to instantiate anything, while a proxy controls access to it and just stand in for the objects doing the real work.
3. How Proxy and Adapter relate?
   Both Proxy and Adapter sit in front of other objects and forward requests to them. Remember that Adapter changes the interface of the objects it adapts, while the Proxy implements the same interface.
   There is one additional similarity that relates to the Protection Proxy. A Protection Proxy may allow or disallow a client access to particular methods in an object based on the role of the client. In this way a Protection Proxy may only provide a partial interface to a client, which is quite similar to some Adapters.
4. What exactly is the “dynamic” aspect of dynamic proxies?
   The proxy dynamic because its class is created at runtime. Think about it: Before your code runs there is no proxy class; it is created on demand from the set of interfaces you pass it.

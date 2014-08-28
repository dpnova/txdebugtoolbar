# txdebugtoolbar

The idea is to have a debug toolbar inspired by django-debug-toolbar and pyramid-debug-toolbar
that gives the power of those tools, but also understands the inherent asyncronous nature of
apps built in twisted.

Initially most testing will be done with cyclone, klein and raw twisted.web. Each of these tools
provides their own way of managing things like template rendering etc, so custom integrations will
be required to framework specific details.

A set of required events to be tracked:

* request accepted
* request dispatched
* response creation
* render start
* render stop
* any outside services - redis/rdbms

The main trick required in this who exercise is that we dont have things like thread locals etc. So we
have do to a bit of magic to find out which request a particular deferred has come from.

My initial feeling about this is to monkey patch Deferred, such that when it is created it
looks back through the stack it has come from to find any relevant requestish or deferredish activity.
Yes, it is not going to be an exact science, but for the purposes of the average web developer we should
be able to provide a fairly compelling overview of their request/response cycle.

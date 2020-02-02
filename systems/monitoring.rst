Monitoring
==========

Whenever we implement something that someone uses, we are said to provide a
service to users. Sometimes services malfunction and users are unable to use
the service.

We call monitoring the process of setting up a system that will notify when
a service is not available to users, so they can fix it.

First stage of monitoring
-------------------------

Inventory
~~~~~~~~~

First, we need to monitor that all services are available. Normally we can start
by making a list of such services. It is also interesting to note the following
characteristics:

* Who is responsible for fixing the service if it becomes unavailable
* Who are the users of the service
* What is the impact to those users if the service is not available
* What is the availability required for the service (e.g. all the time, during
  office hours, only on the last week of a month, etc.)

It is often interesting to perform this inventory in a wider context, taking
into consideration other concerns such as support, backups, etc., but we will
focus exclusively on monitoring concerns in this document.

Note that the inventory for the first stage of monitoring will not include a
significant amount of things which are often monitored, but which are not a
service themselves. We rarely provide a "CPU availability" service, so
monitoring CPU usage does not belong to the first stage of monitoring. Some
people provide server hosting and thus they should monitor whether servers are
operating correctly and available, but if your service is a website running on
those servers, a ping monitor does not belong to the first stage of monitoring.

Those monitors are useful, and sometimes implementing them is relatively cheap
so they can be added early in the monitoring process, but they follow different
design considerations and we will not discuss them until later.

Once we have this inventory, we should start setting up our monitoring. Note
that this inventory will probably be incomplete and that it will require
updates. Every time someone fixes a service which was unavailable, but whose
failure was not detected by monitoring, we should update the monitoring. We
should include updating monitoring to our checklist about putting a new service
into production or modifying it.

Check definition
~~~~~~~~~~~~~~~~

To set up monitoring, normally we use the concept of service checks. A service
check is a process whose output is "available" or "not available".

It is often complex to achieve this. Some services might have built in health
checks that help in this regard, but most do not. Also, a service might be
working correctly, but it might not be available to users for other reasons.

Obviously, if our service check could simulate perfectly users and verify that
all actions a user might perform are working correctly, then we could have a
perfect case. This, or something close to this, actually can happen in some
cases, but in most cases it's impossible or impractical. It is key to find
an adequate check, which is feasible to implement, does not impact the service
itself.

Typically, availability checks should perform requests to the service using its
protocol; for a website we will make an HTTP request. This request should take
into consideration where users of the service are located; while a monitoring
system might run in the same network than the services themselves, often users
are in different networks, so this should be evaluated and it might be
interesting to take action on that. We also need to examine if a single request
is enough to determine availability, or we need to make several; and see what we
should inspect in the response to see if its valid.

Check scheduling
~~~~~~~~~~~~~~~~

Once we have determined the checks we need to perform on our services, we should
schedule them. Check frequency often is limited by the impact of the check on
the service itself (if we perform checks with very high frequency, we might
accidentally perform a denial of service attack on it) and on the monitoring
system itself (as each check consumes resources on the monitoring system and
running too many checks might be problematic), but other than that, we mostly
want to run checks as frequently as possible so we detect problems as soon
as possible.

Besides frequency, we should determine the time periods when checks are run.
Typically this is "all the time". Even if the users do not expect services to be
available all the time, it's often worth checking them at all times, so we can
fix failures before they are noticed- and this also provides useful information.
Sometimes services are expected *not* to be working at some time periods, so in
that case, we might not want to run checks then, although sometimes, a service
running when it's not supposed to is also something we should inspect.

Note that in most monitoring systems, we can schedule checks for some time
period, but limit actions to a different period.

Notifications
~~~~~~~~~~~~~

Finally, to have monitoring we have to decide what to do when a check reports
that a service is not available.

First, in most cases, we should not notify when a check fails. Often it is not
worth reporting every failure; sometimes services are briefly unavailable for
random reasons without really impacting users, or checks themselves might fail
when the service is working correctly. We want to make sure that "enough" checks
have failed to send a notification. This is to be finetuned, but the less checks
we require, the more likely that the notification will be worthless, and if we
require a significant amount of checks to fail, our time to respond to the
problem will grow (and thus the possibility that users will be impacted before
we know and we can fix the problem).

Furthermore, we should consider a combination of three factors:

* Whether the service is expected to be available at that moment
* The status of the person handling the notification at that moment. This can be
  "working" or "not working, but available".

If the service is expected to be available, then we must notify if the person is
available. If the service is not expected to be available, it can be worth
notifying, esp. if the person is working (so they can fix the service before it
is expected to be available)- and in some cases even if the person is just
available.

(Note that to provide services which are available all the time, this "person"
should really be a group of people working shifts, etc. Often, not all time
periods are covered by the same profiles, which has further implications which
will not be discussed here)

Dealing with notifications also has quite a few ramifications; most of them
outside of the scope of this document. However, we will note that:

* Any inaccuracy in notifications must be dealt with, both false positives
  (notifications when services are available) and false negatives (notifications
  not being sent when a service is not available)
* Notifications should be reduced by:

  * Reducing the availability expectation
  * Fixing false positives
  * Improving the reliability of the service

Title: How to keep your embedded linux device up and running
Date: 2018-02-11 12:50:00
Modified: 2018-02-11 12:50:00
Category: FOSDEM 
Tags: FOSDEM, monitoring, embedded, server
Slug: fosdem-2018-talk-01
Authors: Michiel Jordens
Summary: Monitoring embedded devices

[FOSDEM page](https://fosdem.org/2018/schedule/event/linux_up_and_running/)

For this speaker, the user experience was key. He mentioned
an amusing anecdote about residents of california painting 
their lawns green. It is considerably cheaper than keeping
it watered, while the user experience is kept intact.

You cannot be certain of the quality of software uploaded
to the platform. As user experience is key, the user interface
must be kept stable: no crashes or inconsistent UI. 

In the server world, monitoring and keeping things up and running
are daily business. We can take some of their learning and 
apply them to embedded devices.

Some frameworks he mentioned are:

- systemd
- nagios
- Icinga
- Zabbix

These server monitoring tools are very powerful, albeit from a 
server perspective. Trying to run these on an embedded device
will leave you scrounging for resources. Apart from their resource
requirements, they require a central point to which data is uploaded.
This is another reason why we cannot copy and paste these tools.

To solve these 2 issues we shall: store the data locally and
focus on passive checks. Passive checks are more lenient towards
resources. An example of a passive check is to only check RAM
usage with a granularity of 100MB. This frees up resources,
at the expense of some contextual data.

There is still another issue left to solve. It makes no sense to wake
IoT devices for state introspection as most IoT devices are in sleep mode.
This was fixed by developing **faultd**.

The basic mode of operation for faultd is as follows:
listen for changes -> analysis by the decision maker -> act.

#### Listeners
- systemd
- audit

audit reports on max usage based on rlimits. This generates
a high overhead on ARM devices, so an alternative was made:
**rlimits-events**.

#### Decision maker
*for diagrams, please refer to the powerpoint presentation on the fosdem website*

The decision maker can generate a report and store the data
into a database. Knowing which application is causing issues,
is vital information.

#### Actions
- Recover service
- Restart service
- Reboot
- Reboot recovery

### Sumary
This was a very interesting talk. Applying knowledge from one domain
onto another shows how you can learn from each other. 

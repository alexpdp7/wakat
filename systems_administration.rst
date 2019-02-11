Systems administration
======================

Most companies have dedicated systems administrators (and among them, there are specialists; virtualization, networks, etc.).

However, even if you don't intend to become a systems administrator, systems administration skills are useful.

If you are a developer, the software you develop will need to run somewhere. Often this won't be your duty, but even if it isn't, it's useful to have knowledge about this area. Also, developers need some infrastructure (source control servers, continuous integration software, etc.), so knowledge running them is useful

Also, on "real life", running your own services can make sense.

Like coding, a good way to learn these skills is to pick projects which are useful to you.

Storage is often a good topic. There are many services available which provide storage, but personal storage needs have grown lately. Phones with decent cameras and other devices often mean that we can have gigabytes of pictures to store. Video takes even more space, making terabytes of data a need for many.

At those scales, sometimes it can make sense to build your own. Hard drives of a couple of terabytes of capacity are affordable and some hosting provides offer relatively cheap dedicated servers with terabyte hard drives.

To figure out a price, some study is required. There are two often-overlooked aspects of storage for domestic purposes:

* Backups. If your data is valuable, you don't want to lose it due to problems such as hardware failure or human error. Given that we often want to keep historical data and not just a copy of the most current set of data (as you might want to recover deleted files), you will need more backup capability than the data itself.
* RAID. RAID is a technology which increases the *availability* of your storage. In its simplest form, two hard drives are treated a single one, allowing one to fail while still being able to access your data.

Note that RAID does not substitute backups. RAID will allow you to continue accessing your data in the event of hardware failure without having to restore your backups, which can be very significant for two reasons; terabytes of backups cannot be recovered quickly and if you don't have spares for the failed hardware, you will need to acquire it and often, reinstall stuff.

If you have to choose one, choose backups. If you can do both. that's better.

The other decision is where to host your storage.

Hosting it in your home has several advantages:

* Running costs are basically electrical power, which tends to be a small part of the total cost
* Access within a local network will typically be much faster than over the Internet, unless you have gigabit Internet

Hosted providers have their own advantages:

* Availability will be better; datacenters have redundant systems which means that they rarely have outages of any kind (power, Internet connectivity, etc)

A combination of the two can be useful- esp. you can choose different approaches for main storage and backup storage.

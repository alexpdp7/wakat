Writing good code
=================

The first step to knowing how to write good code is knowing what is good code.

First and foremost, good code is the code which does what it's asked for. The people doing the asking are ultimately answering the question- does this do what we need and what we paid for? You need to identify them, learn about their needs and figure out the software requirements.

It is not uncommon for the people requiring the software to not have a clear view of the requirements. They hopefully know their way around the activity they require software for, but articulating the requirements exactly is not exactly trivial; requirements must be formal and rigorous, and that requires an effort which people do not realize is needed.

Outside the requirements, most software has additional requirements which are often not stated explicitly or not correctly measured. Those include:

* Easy to use: software exists to fulfill a need. If it it's too hard to use, it will not be used or it will cause more problems than it solves. One has to be aware of the people who will be using the software and take them into account while writing the software.
* Robust: software fails, and each situation has its own implications for failure. Even minor bugs in software deployed in a space satellite can have terribly costly consequences, while sporadic, low frequency bugs in non-critical software can often even go unnoticed with no consequences.
* Low cost of development: software has a cost, and it sometimes can offset the benefits it generates.
* Efficient: software should solve the problem it is intended for in a reasonable time and with a reasonable amount of resources. A real-time trading system which makes excellent choices but takes days to find them might be completely worthless. A text editor which requires a massively powerful system to work correctly is not desirable.
* Easy to maintain/extend: software is rarely static. New requirements tend to appear continuously and often, tackling them is more expensive than necessary due to previous choices. 
* Easy to deploy: this is often overlooked, but for software to be useful it needs to be deployed. Excessive requirements or arcane deploy procedures can hinder (or even stop) the software from being put into production and thus prevent it from being useful.

We will describe some techniques to help with delivering requirements.

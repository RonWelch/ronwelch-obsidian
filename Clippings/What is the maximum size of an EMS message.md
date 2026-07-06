---
title: What is the maximum size of an EMS message?
source: https://support.tibco.com/external/article/89598/what-is-the-maximum-size-of-an-ems-messa.html
author:
  - "[[CSG Inc.]]"
published: 2017-01-14
created: 2026-07-01
description:
tags:
  - clippings
  - Tibco
---
What is the maximum size of an EMS message? 
![[Pasted image 20260701145647.png]]
#### Article ID: KB0089598

calendar\_today

#### Updated On: 01-14-2017

| Products | Versions |
| --- | --- |
| TIBCO Enterprise Message Service | \- |
| Not Applicable | \- |

#### Details

**Resolution:**  
Description:  
\= = = = = =  
The maximum size of an EMS message.  
  
Solution:  
\= = = = =  
In general, the maximum possible size of an EMS message would be around 512 MB. However, sending such big messages is not recommended. If you have to send a large message, it would be better to slice it in smaller chunks before sending it and reassembling it at the receiving end.  
  
The following are a few things you need to take into consideration if you need to send large messages.  
  
1). The problem with a large message is that the sending and receiving applications will require more memory. For instance, a topic subscriber prefetches 64 messages (default), which means that when the application is receiving a message and processing it, TIBCO EMS will send pending messages in the background to the application. If messages are large, the memory usage of the application could increase dramatically. For queue, the prefetches size can be configured. You may want to consider turning the prefetch feature off (with the value of NONE).  
  
2). Is the application sending large messages across a WAN or sending/receiving locally in a LAN or across subnets?  
  
3). Compression may help. Be aware that compressing/uncompressing messages takes time and memory. The application would need at least twice the size in memory of the message to compress/uncompress it. Additionally, make sure that each computer is able to handle an excess load (NIC speed, link bandwidth, memory available).  
  
4). If a link fails and the message is lost, the message will be resent although the application may not receive it again as the receive() call did not yet return a message. This is the major reason large messages should not be used because the whole message would have to be resent.  
  
Again, large messages should be split into smaller messages and they should be sent to a queue. A dedicated queue receiver would then reconstruct the message by concatenating the group of smaller messages into a large one.  
  
5). Run some tests with alternate values set for max\_msg\_memory to confirm that this parameter is large enough in the server. The default value is 0 (zero) which indicates no limit.  
  
6). For queues, maxbytes defines the maximum size (in bytes) of all messages that can be waiting in the queue. By default, or if maxbytes is set to 0, there is no limit to the size of a queue. If a receiver is off-line for a long time, maxbytes limits the memory allocation for the receiver's pending messages. Messages that would exceed the limit will not be accepted into storage and an error is returned to the message producer. For topics, maxbytes limits the total size (in bytes) of all messages waiting for delivery to each durable subscriber on that topic. This limit applies separately to each durable subscriber on the topic.  
  
7). Additional performance testing should be done if flow\_control is enabled.

#### Summary

What is the maximum size of an EMS message?

Was this article helpful?

thumb\_up Yes

thumb\_down No
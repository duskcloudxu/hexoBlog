---
title: AWS Free Tier Limit Alert
date: 2020-01-19 11:09:25
tags:
- aws
---

> TL;DR:
>
> Your account status of different region is independent, therefore if you received the free tier limit alert and you thought you should only have one instance, it's highly possible that you launched another instance/instances earlier in **Other** region that different from your current region. Try to switch region at the top bar of your AWS console and check you EC2 status in other regions.

Recently I am learning MIT 6.828 OS online, and using an EC2 instance of AWS as the linux environment for the lab and homework of that course. I remember I had an instance running on EC2 earlier for another course in USC, however, when I logged into my account, that instance was gone. So I just registered another instance of ubuntu and began doing the homework.



MIT 6.828 is very hardcore and I learnt that do not mess with operating system if you got other choices. But it's not what I want to cover in this blog. Days earlier I receiving an Email from AWS saying that my free tier hour is running out, and that is why I write this blog.



After received that information, I checked my AWS console again and quite sure that I only have one EC2 instance. After some research, I found that the status of your account in different region is independent, so I switched to another region of US-east, and found my forgotten EC2 instance there and stopped it.

 


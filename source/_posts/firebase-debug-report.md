---
title: firebase debug report
date: 2019-10-15 02:43:44
tags:
- firebase

---

> It is really a waste of my time.

Recently I was working on backend of my group project, which is mainly based on firebase. Our group project is to build a web app as an interface of a dataset with some data visualization features. So I started to design the project structure, and parse the dataset into JSON file in local by using python. I encountered several problems when I was attempting to upload the dataset JSON file, which is of 400MB, to the firebase. But it was not a very big deal, I found a open source tool called `firebase-import` and successfully uploaded it online. I thought the remaining work gonna be easy since it's just building frontend and use firebase API, but surely I was wrong, it was just the beginning of my misfortune.



The first problem was the indexing. Obviously you need pagination for the performance of your application, and there is the first question: Firebase is not equipped with any index-based query. The only way to make a range query is to order the database by value of keys or children and use `startAt` and `endAt` and similar methods. The limited query methods means that an index value should be there in every level of your database structure, otherwise you might could not found any value to make a range query when you implementing CRUD operations in your application. Also, the guide of Firebase real time database is problematic. When I was attempting range query by using `orderByChild`, I always receive thousands of records, and that, surely, crushed my web app. However, there is no obvious mistake in query language, everything is just as fine as official example. Therefore, there must be something I missed one the FM. After an hour of self-doubting, I found the answer from a small corner of official manual, which is, you need to define the key you want to use in any `orderby` query in the rules.  Therefore, RTFM is really important, although the manual would be really hard to understand and it arranges its content in a irregular way.
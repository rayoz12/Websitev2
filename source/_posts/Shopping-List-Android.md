---
title: 'Adventures in Android: The Shopping List'
tags:
  - Android
  - Java
  - Dart
  - Flutter
  - Kotlin
  - NestJS
  - Articles
banner_img: /projects/img/ShoppingTrolley/shoppinglist.jpg
index_img: ./projects/img/ShoppingTrolley/shoppinglist.jpg
excerpt: >
  My family had a particular problem with buying things, so I decided let's make
  it better.
categories:
  - - Android
  - - Java
  - - Dart
  - - Flutter
  - - Kotlin
  - - NestJS
  - - Articles
date: 2020-09-24 15:24:29
---

# Adventures in Android: The Shopping List
There's two reasons I like to write software:
1. Interesting and novel concepts, and
2. Making mine and other's lives easier with it.

This project firmly fits in the second category because my family can't go shopping
without calling up multiple times or just flat out forgetting things (including me of course).
I wanted to write an application that just meant would be juggling the app instead of a phone call
and the basket (and juggling fruits at the same time 😅).

In general I also wanted to learn about proper Android development.

This article details 3 approaches I took to solving this problem and how the problem evolved over time.

## 1. A simple solution to a simple problem

If you think about it, a shopping list can be abstracted into a simple to do list. 
There's something you need to do (get an item) and once done mark it off as done (the item has been bought).
People in my family would be able to add items and whenever me and dad would go to store we can check the list and mark off items.
Someone at home would be able to see the items being marked off and be confident that that evening it would be home.

Following this I wanted to solve the problem so over a few days I wrote an Android App and a web sever
to back it.

- Android App: https://github.com/rayoz12/LiveShoppingList
- Web Server: https://github.com/rayoz12/LiveShoppingListServer

### Android App: The First Iteration
When I started out Kotlin was a small language in the Android ecosystem, compared to the behemothic
that we see it as today. I opted for Java solving seeing how established it was and I knew in the
future I would have to use it to program the Android based Robot. (See: {% post_link Smart-Restaurant-System %})

Particular to my learning I wanted to see how Android worked with REST APIs so in particular I looked at
HTTP & JSON libraries. I finally decided on:
- OkHttp
- GSON (Google's JSON library)

![The first simple application](first_app.jpg)

This application was extremely simple:
- User's could add items and the app would tell me who added it.
- An optional quantity on the item.
- Comments on the Item in case the name wasn't clear enough.
- The web server backing this was a 150 line express server with a file JSON database.
  - This wasn't really the focus so I just wrote a quick and dirty backend
  - For connection from anywhere I hosted the web server on Azure

This version of the application was simple and it proved useful to my family, we used for quite it while.
I hosted the web application on Azure until my student credit ran out and I decided it was time to
pay a little more attention to polishing this app. 

## The Slow Evolution
As we continued using the app it worked well, but as time went there were three issues on my mind:
1. It was only a single list
    - Apart from just the list for my families groceries I wanted to also have a list of tech I wanted to buy
    - A quick band-aid solution was to use a hardcoded API key
    - There was no categorisation of any form for items making large lists hard to navigate.
2. No Security of any kind.
    - When I wrote this app the focus I had in mind was learning and useability.
    - I didn't need an authentication system so I didn't include it.
3. Lack of scalability to other Families
    - There's only one list
    - No authentication system to identify users and their family

## 2. Addressing the Flaws: Kotlin & NestJS
2 years after running with the App I decided it was time to see how Android development had changed.
I began looking into Kotlin as well as the new way of structuring applications.

> It's worth mentioning at the time Android (and Google in general) had gotten the reputation of 
> deprecating features and software of Android in short time frames. In a way it felt like Android
> Development had turned into web development with rapid changes.

### Kotlin, Single Activity Architecture, Navigation, User Repositories and more!

The Android Landscape had changed dramatically since I last looked there was a lot of buzzwords
and frameworks that had been released. Namely:
- Kotlin
- Single Activity Architecture (using fragments rather than activities)
- AndroidX Navigation
- Dependency Injection (DI)
- User Repositories
- View Binding

![Architecture](rec_arch.png)
<div style="text-align: center; color: grey">The Recommended Google Architecture</div>

Compared to my previous experience these frameworks were pretty exciting with the possibility to make
a whole lot easier. In addition I wanted to concentrate on the design of the application to make it
more aesthetically pleasing. Here's the output of dabbling in Android for 2 weeks:

{% raw %}
<div class="container">
  <div class="row">
    <div class="col-md-3">
        <a target="_blank" href="v2/login.jpg"><img src="v2/login.jpg" alt="login"></a>
    </div>
    <div class="col-md-3">
        <a target="_blank" href="v2/select_fam.jpg"><img src="v2/select_fam.jpg" alt="Select Family"></a>
    </div>
    <div class="col-md-3">
        <a target="_blank" href="v2/select_list.jpg"><img src="v2/select_list.jpg" alt="Select List"></a>
    </div>
    <div class="col-md-3">
        <a target="_blank" href="v2/list.jpg"><img src="v2/list.jpg" alt="Shopping List"></a>
    </div>
  </div>
</div>
{% endraw %}

Available at this link:
https://github.com/rayoz12/LiveShoppingList/tree/v2

The tools I mentioned [above](#Kotlin-Single-Activity-Architecture-Navigation-User-Repositories-and-more)
were all used in writing this app as it was mostly about a learning endeavour. Here are my thoughts about each:

| Technology                      	| Thoughts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             	|
|---------------------------------	|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| Kotlin                          	| Kotlin is an excellent language and easy to pick up. The language itself has excellent tools. There's definitely some nice syntax and tools that I didn't get a chance to play around with.  Coroutine's are a perfect replacement for AsyncTask and when used with Retrofit it was so easy to integrate them together. Moshi & Retrofit is one of the easiest JVM language based libraries I've used.                                                                                                               	|
| Single Activity Architecture    	| In my opinion Single Activity Architecture along with Fragments are one nicer ways to handle navigation in app. In the past I've had many activities but it was hard to transfer state across activity boundaries and the easy was to fall back to another HTTP request.  AndroidX Navigation is also an excellent tool in achiveing this architecture. Fragments have come a long way since I last used them in 2014 for a school assignment.                                                                        |
| AndroidX Navigation             	| Navigation and it's integration with Android Studio is extremely convenient. Having named routes and being able to see navigation paths within the app was great. It also makes it easier to approach Single Activity Architecture.                                                                                                                                                                                                                                                                                  	|
| Dependency Injection (via Koin) 	| Dependency Injection was a tool I was always look out for especially since working with Angular. However it has it's place as an app scales, in a small simple application I only ever injected the ViewModel between fragments, but I'm sure there are many use cases to inject other classes.  If you're starting a new application I would recommend looking at Dagger as well and comparing the two, but Koin can definitely do what it needs to.                                                                	|
| ViewModels                      	| View Models are one the best things to come to Android, they provide a place to store data that doesn't get lost when the activity needs to be recreated. I highly recommend that you use ViewModels with a dependency injection library to make it easier to get application state from anywhere. This is such a useful tool for apps at any scale / complexity because it makes it so much easier to handle application state. I would also look towards using AndroidX Live Data with ViewModels and ViewBinding. 	|
| Data Repositories               	| This is useful I think only as an app scales up. I was finding hard to justify it being in my small app. My understanding is this is an abstraction over multiple data sources for example persistent user data on device and user data from server. However in my case it wasn't much use because I always relied on the server as one data source. Think carefully before including this in your application.                                                                                                      	|
| View Binding                    	| View Binding is something that I've been looking to be on Android for a long time and I recommend anyone to use it when they can. The only downside is that it uses code generation and you need to keep it in mind when you write and debug your app.                                                                                                                                                                                                                                                                   	|

I got pretty far as you can see with the screen shots but I found it difficult to work with nested
recycler views. Just have a look at the code for the List view where I need to nest the horizontal nav bar
and item list.

### NestJS
The server needed to also support all these new features, the one line server and JSON file won't do
anymore. So I moved over to NestJS & an SQLite DB. NestJS also supports swagger for nice API
documentation. It supports authentication and a better level of security out of the box.
I'm sure you can find one my other articles where I talk about NestJS.

This post will be continued next week where I talk about Flutter and Dart!

Stay tuned too because all of this android knowledge isn't for naught. Eventually my goal is make an
app for my car. It won't be as good as the Tesla App ofc but it'll be able to control a whole lot
of things!

















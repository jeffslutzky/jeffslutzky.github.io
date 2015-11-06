---
layout: post
title: "Refactoring Your Code After Adding a Feature"
date: 2015-11-05 19:08:57 -0500
comments: true
categories: "Flatiron&nbsp;School"
---
This has been project week at Flatiron, and while designing our websites we've been learning to follow the principle of <em>minimum viable product</em>, or <em>MVP</em>. This <a href="http://leanstack.com/minimum-viable-product/">basically means</a> "the smallest thing you can build that lets you quickly make it around the build/measure/learn loop" or "the smallest thing you can build that delivers customer value (and as a bonus captures some of that value back)."

The idea is to start by building a simple product that works before adding new features. Here's the image we were shown in class that illustrates MVP:

<img src="/images/how-to-build-a-minimum-viable-product.jpg">

Our group decided to build a site that maps out public restrooms in New York City so users can easily find a place to answer nature's call. Users can review existing restrooms and even add restrooms to the site's database. In creating our site, we found it useful to follow the MVP principle, but this created some issues we had to fix later on.

There's an <a href="https://data.cityofnewyork.us/Recreation/Directory-Of-Toilets-In-Public-Parks/hjae-yuav">existing database</a> of public restrooms in New York City public parks, so we decided that our MVP was to build a map showing all these restrooms. Only after we'd built out this minimum product and had it working would we build the next feature: letting users add restrooms to the database.

<img src="/images/mvp_boat.jpg"><br>
<a href="http://transformcustomers.com/minimum-viable-product-how-it-helps-time-to-market/">credit</a>

When we built out our MVP, we created a table in our database called <em>public_parks</em>. We also had a PublicParks controller, a PublicParks model, PublicParks views, and PublicParks routes. We had a <em>public_park_id</em> as a foreign key in another of our tables, and we had <em>public_parks</em> objects throughout our code. This was all fine when we were working on our MVP, but it created problems when we expanded our project to let users add public restrooms in other types of venues such as restaurants and stores. Each of our parks had a "show" page with the path "/public_parks." But this URL wouldn't make sense for a restaurant or bookstore. And anyone looking at our code would be confused by a <em>public parks</em> table that also included stores and restaurants, or <em>public_park</em> objects that weren't parks. What to do?

We realized we had a couple of options. One, we could create a new table called <em>location types</em> and refactor from there. The other option was just to give the <em>public parks</em> table, files, objects, and routes a new name. Which choice was better?

<img src="/images/cookie.jpg"><br>
<a href="https://twitter.com/thirdmanlabs/status/445985883414872064">credit</a>

We realized that, given the features we were planning, there was no need to create a separate table of location types. There was nothing that separated one type of location from another: whether the restroom was in a park, store, or restaurant, our users would be able to rate it on a scale from 1 to 10 and write a review. Therefore, we decided to rename our <em>public parks</em> table, controllers, views, models, and routes as <em>restrooms</em>. Had we wanted to add any features that applied only to stores or restaurants -- such as operating hours -- it might have made more sense to create separate location types. But since we were creating a fairly simple website, it made more sense to put every location in one table and not differentiate.

But we actually <em>did</em> add a column for location type to our parks table, which gets assigned when a user adds a restroom to the site. The user selects a choice from a dropdown table to categorize the restroom as a park, restaurant, or store. If, in the future, we decide to do different things for these different location types, we have a way to distinguish them without tediously going through all our locations and assigning them location types, one by one.

Depending on your MVP and the features you expect to add later, you'll have to figure out the best way to structure your code from the start and what you'll have to do to refactor your code later. As I go through my career and work on more complicated projects, I look forward to learning more about how to plan my code and refactor it. Revising code can be just as fun and challenging as writing it.

<img src="/images/tree.jpg"><br>
<a href="https://www.pinterest.com/pin/433893745324229364/">credit</a>


---
layout: post
title: "Azure Tips and Tricks Part 66 - Using the Data Migration Tool with Cosmos DB"
excerpt: "Learn how to use the Data Migration Tool with Cosmos DB"
tags: [azure, windows, portal, cloud, developers, tipsandtricks]
share: true
comments: true
---

## Intro

Most folks aren't aware of how powerful the [Azure](http://www.azure.com) platform really is. As I've been presenting topics on Azure, I've had many people say, "How did you do that?" So I'll be documenting my tips and tricks for Azure in these posts.

## The Complete List of Azure Tips and Tricks

[Available Now!](https://michaelcrump.net/azure-tips-and-tricks-complete-list/){: .btn .btn--success} 

## Using the Data Migration Tool with Cosmos DB

One tasks that seems to come up over and over is migrating data from one format into another. I was recently building out an API and needed to dump some data into Cosmos DB. The tool that made short work of this was the [Azure DocumentDB Data Migration Tool](https://www.microsoft.com/en-us/download/confirmation.aspx?id=46436). In my case, I needed to dump a large JSON file into Cosmos DB. Here is how I did it. 

## The Tools + Sample Data

Download and install the [Azure DocumentDB Data Migration Tool](https://www.microsoft.com/en-us/download/confirmation.aspx?id=46436)

Grab whatever sample file that you'd like to experiment with. I'm using the `en_kjv.json` JSON file from [here](https://github.com/thiagobodruk/bible/tree/master/json)

Now we're ready to begin work! 

## Get to Work

Open the Data Migration Tool and under **Source Information**, point to the local JSON file as shown below. 

<img style="border:3px solid #021a40" src="/files/migrationcosmos1.png">

Ensure you have a Cosmos DB database id and collection. I'm using the following: 

<img style="border:3px solid #021a40" src="/files/migrationcosmos7.png">

Go to **Keys** (inside your Cosmos DB blade in the portal) to copy the **Primary Connection String**

<img style="border:3px solid #021a40" src="/files/migrationcosmos2.png">

You'll need to append the Database name to the end of the string. For example `Database=bible` will be appended to the string `AccountEndpoint=https://mbcrump.documents.azure.com:443/;AccountKey=VxDEcJblah==;Database=bible` that I copied out of the portal. Now press **Verify Connection**. 

<img style="border:3px solid #021a40" src="/files/migrationcosmos3.png">

You'll need to add the **Collection** and in my case it is `verses`. We'll take the defaults on the next two screens and you'll finally see a **Confirm inport settings** page. 

<img style="border:3px solid #021a40" src="/files/migrationcosmos4.png">

You can even click on **View Command** to see the command that will be used to migrate your data. This is helpful to just learn the syntax. 

You'll finally see the Import has completed with 66 transferred. (This is the total number of books in the Bible.)

<img style="border:3px solid #021a40" src="/files/migrationcosmos5.png">

If you go back to the Azure Portal and open Cosmos DB and look under **Data Explorer**, you'll see the data has been imported successfully into our collection. 

<img style="border:3px solid #021a40" src="/files/migrationcosmos6.png">

We were up and running in just a matter of minutes! Awesome!

## Want more Azure Tips and Tricks?

If you'd like to learn more Azure Tips and Tricks, then follow me on [twitter](http://twitter.com/mbcrump){: .btn .btn--success} or stay tuned to this blog! I'd also love to hear your tips and tricks for working in Azure, just leave a comment below. 
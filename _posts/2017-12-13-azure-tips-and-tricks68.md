---
layout: post
title: "Azure Tips and Tricks Part 68 - Access Cosmos DB through a .NET Application"
excerpt: "Learn how to access Cosmos DB through a .NET Application"
tags: [azure, windows, portal, cloud, developers, tipsandtricks]
share: true
comments: true
---

## Intro

Most folks aren't aware of how powerful the [Azure](http://www.azure.com) platform really is. As I've been presenting topics on Azure, I've had many people say, "How did you do that?" So I'll be documenting my tips and tricks for Azure in these posts.

## The Complete List of Azure Tips and Tricks

[Available Now!](https://michaelcrump.net/azure-tips-and-tricks-complete-list/){: .btn .btn--success} 

## Access Cosmos DB through a .NET Application

I use .NET to access my Cosmos DB instance a lot and typically before spinning up an ASP.NET MVC application (for instance), I use a console application to explore objects and play with different settings. In this brief walkthrough, I'll show you how I access it and provide a "template" that you can use for your own explorations. Keep in mind that this is just how I work with Cosmos DB, you may have a different (or better) way. 

I've pushed some code to my GitHub repo that you can clone and it can be found [here](https://github.com/mbcrump/cosmosdb). While you can work with Cosmos DB with whatever IDE or editor that you choose, I'll work with Visual Studio. Begin by opening Visual Studio and selecting **File** -> **New** -> **Project from Existing Code** and paste in the [git url](https://github.com/mbcrump/cosmosdb.git) and press **Clone**.

<img style="border:3px solid #021a40" src="/files/clonerepoazure1.png">

The solution should now be loaded. Head over to the `app.config` file and you'll see the following `appSettings` tag: 

```text
  <appSettings>
    <add key="endpoint" value="enter" />
    <add key="authkey" value="enter" />
    <add key="database" value="enter" />
    <add key="collection" value="enter" />
  </appSettings>
  ```

  Here you'll simply plug in the values from Keys (inside your Cosmos DB blade in the portal) for example: 
  
  * endpoint = URI
  * authkey = Primary Key

  <img style="border:3px solid #021a40" src="/files/clonerepoazure2.png">

You can go to **Data Explorer** for the following pieces: 

* database = db name
* collection = collection name

<img style="border:3px solid #021a40" src="/files/clonerepoazure3.png">

Now that your `app.config` is setup properly, you'll need to make minor tweaks to the code, unless you are using the data that I used in my [earlier post](https://www.michaelcrump.net/azure-tips-and-tricks66/).

## Walking through the demo application

We declare each value as a string that we can later use as well as instantiate your DocumentClient object. 

```csharp
private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
private static readonly string EndPointId = ConfigurationManager.AppSettings["endpoint"];
private static readonly string AuthKeyId = ConfigurationManager.AppSettings["authkey"];

private static DocumentClient client;
```

We pass the endpoint and authentication key and then check to see if the database and collection exist (code not shown here). If everything is OK, we call the CreateDocumentQuery method and pass our database and collection to it. We then loop through each item and display whatever piece of the collection that we want. 

```csharp
client = new DocumentClient(new Uri(EndPointId), AuthKeyId);
CreateDatabaseIfNotExistsAsync().Wait();
CreateCollectionIfNotExistsAsync().Wait();

FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

IQueryable<dynamic> bookQuery = client.CreateDocumentQuery<dynamic>(
UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), queryOptions);
                    
Console.WriteLine("Running query...");
foreach (dynamic book in bookQuery)
{
    Console.WriteLine("\tRead {0}", book.book);
}
           
Console.Read();
```

**Why did you use Dynamic?** I used a dynamic type because the structure of your collection will be different everytime. It would be best to stub out a class and add properties for each field in your collection. There are example online of how to do that. 
{: .notice--primary}


In this instance, I am just displaying all the book of the Bible. 

<img style="border:3px solid #021a40" src="/files/clonerepoazure4.png">

You could add a `.Where` clause and many more. As stated earlier, I typically use this for exploring and trying different settings as shown below: 

<img style="border:3px solid #021a40" src="/files/clonerepoazure5.png">

Again, just one way that I work with Cosmos DB. I hope this helps! 

## Want more Azure Tips and Tricks?

If you'd like to learn more Azure Tips and Tricks, then follow me on [twitter](http://twitter.com/mbcrump){: .btn .btn--success} or stay tuned to this blog! I'd also love to hear your tips and tricks for working in Azure, just leave a comment below. 
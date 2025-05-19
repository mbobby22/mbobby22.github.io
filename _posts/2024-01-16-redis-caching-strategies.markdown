---
layout: post
title:  "Caching strategies"
date:   2024-01-16 12:00:00 +0200
categories: coding redis caching
excerpt: "There are at least 3 major popular caching strategies for you to always consider: Lazy caching, Write-through, Time-to-live."
---

There are at least 3 major popular caching strategies for you to consider, depending on the type of data that needs to be cached and the way we access it, as follows:

<h3>Lazy caching</h3>

<strong>Populate the cache only when an object is actually requested by the application:</strong>

Your app receives a query for data, for example the top 10 most recent news stories.
Your app checks the cache to see if the object is in cache.
If so (a cache hit), the cached object is returned, and the call flow ends.
If not (a cache miss), then the database is queried for the object. The cache is populated, and the object is returned.

*Example*: For data that is going to be read often, but written infrequently. User updates profile less times, but the profile info might be accessed dozens or hundreds of times a day, depending on the user.


<h3>Write-through</h3>

<strong>Updated in real time when the database is updated:</strong>

If a user updates his or her profile, the updated profile is also pushed into the cache.

It is typically updated by a specific piece of application or background job code.

*Example*: any type of aggregate, such as a top 100 game leaderboard, or the top 10 most popular news stories, or even recommendations


<h3>Time-to-live</h3>

<strong>Apply a time to live (TTL) to all of your cache keys (except write-through)</strong>

*Example*: rapidly changing data such as comments, leaderboards, or activity streams

{% highlight json %}
[{"rule":"user_field_skype","data":{"text":"lorem"}}]

[{"rule":"user_field_member_subscription","data":{"choices":["0"]}}]

[{"rule":"user_field_member_subscription","data":{"choices":["digital"]}}]
{% endhighlight %}


<h2>The performance impact of "Russian doll" caching</h2>

Russian Doll Caching is a caching technique used in Ruby on Rails to optimize the performance of web applications by `caching multiple nested fragments of a view`. It’s called *“Russian Doll”* caching because the nested fragments are similar to Russian nesting dolls, where one doll is contained inside another.

While Russian Doll Caching can significantly improve the performance of a Rails application, there are some potential [risks and challenges][russian-doll-doc]{:target="_blank" rel="noopener"} that developers should be aware of:

<h4>Over-caching</h4>

<h4>Cache invalidation complexity</h4>

<h4>Increased memory consumption</h4>

<h4>Debugging difficulties</h4>

<h4>Cache stampede</h4>

Fragment caching is most beneficial when used in scenarios where rendering parts of a view is expensive in terms of time or resources. Some optimal times to use fragment caching include:

- Complex views
- Static or infrequently updated content
- Reusable components
- Content with different update frequencies
- Expensive third-party API calls

See Basecamp's [Russian Doll usage results][russian-doll-sample]{:target="_blank" rel="noopener"} from Basecamp with memcached.

[russian-doll-doc]: https://patrickkarsh.medium.com/rails-caching-patterns-russian-doll-caching-dca569d04fe0
[russian-doll-sample]: https://signalvnoise.com/posts/3690-the-performance-impact-of-russian-doll-caching

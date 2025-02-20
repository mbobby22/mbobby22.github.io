---
layout: post
title:  "REST error codes"
date:   2024-03-20 09:21:00 +0200
categories: coding api rest
excerpt: "HTTP specification defines these standard status codes divided into five categories that can be used to convey the results of a client’s request."
---

I have always wondered what codes should I cover by my endpoints, since requirements are not always covering this.

HTTP specification defines these standard status codes divided into [five categories][http-status-code]{:target="_blank" rel="noopener"} that can be used to convey the results of a client’s request:

{% highlight quote %}
1xx: Informational – Communicates transfer protocol-level information.
2xx: Success – Indicates that the client’s request was accepted successfully.
3xx: Redirection – Indicates that the client must take some additional action in order to complete their request.
4xx: Client Error – This category of error status codes points the finger at clients.
5xx: Server Error – The server takes responsibility for these error status codes.
{% endhighlight %}

I guess we should include ar least one code from each of [these categories][http-status-code]{:target="_blank" rel="noopener"} so that consumers would know what happened with their request.

[http-status-code]: https://restfulapi.net/http-status-codes

---
layout: post
title:  "Load balancing setup: Nginx Haproxy Varnish"
date:   2019-03-05 13:30:00 +0200
categories: devops architecture load balancing nginx haproxy varnish
---

<h2>Load balancing methods</h2>

With a `round-robin` scheme, each server is selected in turns according to the order you set them in the config file. This balances the number of requests equally for short operations.

`Least connections-based` load balancing is another straightforward method. As the name suggests, this method directs the requests to the server with the least active connections at that time. It works more fairly than round-robin with applications where requests sometimes take longer to complete.

In a server setup where the available resources between different hosts are unequal, favouring some servers over others might be desirable. Defining `server weights` allows you to fine-tune the load and further balance it. The server with the highest weight in the load balancer is selected the most often.

<h3>Nginx</h3>

To [setup Nginx as a load balancer][nginx-samples]{:target="_blank" rel="noopener"} for backend servers, follow these steps:

1. Open the Nginx configuration file with elevated rights
2. Define an upstream element and list each node in your backend cluster
3. Map a URI to the upstream cluster with a proxy_pass location setting
4. Restart the Nginx server to incorporate the config changes
5. Verify successful configuration of the Nginx load balancer setup

<h4>Sample upstream</h4>

{% highlight config %}
upstream samplecluster {
  server localhost:8080;
  server localhost:8090;
}
{% endhighlight %}

<h4>Reverse proxy</h4>

{% highlight config %}
location /sample {
  proxy_pass http://samplecluster/sample;
}
{% endhighlight %}

<h4>Weight a load balanced server</h4>

{% highlight config %}
upstream samplecluster {
  server localhost:8090 weight=10;
  server localhost:8080 weight=20;
}
{% endhighlight %}

<h4>Stick sessions</h4>

{% highlight config %}
upstream samplecluster {
  ip_hash;
  server localhost:8090 weight=10;
  server localhost:8080 weight=20;
}
{% endhighlight %}

<h4>Remvoe a server that is offline</h4>

{% highlight config %}
upstream samplecluster {
  ip_hash;
  server localhost:8090 weight=10;
  server localhost:8080 weight=20;
  server localhost:8070 down;
}
{% endhighlight %}

More here about configuring an [nginx load balacing setup][nginx-link]{:target="_blank" rel="noopener"}.

<h3>Haproxy</h3>

HAProxy (High Availability Proxy) is a TCP/HTTP load balancer and proxy server that allows a webserver to spread incoming requests across multiple endpoints. This is useful in cases where too many concurrent connections over-saturate the capability of a single server. Instead of a client connecting to a single server that processes all of the requests, the client will connect to an HAProxy instance, which will use a reverse proxy to forward the request to one of the available endpoints, based on a load-balancing algorithm.

<h4>Install</h4>

{% highlight bash %}
sudo apt-get install haproxy
{% endhighlight %}

<h4>Config</h4>

> File: /etc/haproxy/haproxy.cfg

{% highlight config %}
frontend haproxynode
    bind *:80
    mode http
    default_backend backendnodes
{% endhighlight %}

{% highlight config %}
backend backendnodes
    balance roundrobin
    option forwardfor
    http-request set-header X-Forwarded-Port %[dst_port]
    http-request add-header X-Forwarded-Proto https if { ssl_fc }
    option httpchk HEAD / HTTP/1.1\r\nHost:localhost
    server node1 192.168.1.3:8080 check
    server node2 192.168.1.4:8080 check
{% endhighlight %}

<h4>Run and Monitor</h4>
{% highlight bash %}
sudo service haproxy restart
{% endhighlight %}

See complete [Haproxy load balancing setup][haproxy-link]{:target="_blank" rel="noopener"}.

<h3>Varnish</h3>

**HAProxy** is a `reverse-proxy load balancer` and **Varnish** is a `reverse-proxy cache`. Both these tools are open source and offer great outcomes when it comes to performance, scaling, and resilience. Suggested setup:

1. HAProxy servers can be running in master-slave mode `receiving requests` from clients and load balancing them to a set of servers in the backend in a round robin way.

2. `Caching layer` can be Varnish servers for *caching static web content* like *images, gifs, css, js files, etc*. This ensures quick delivery of web content and saves making calls to the web server asking for content every time we need static content to be delivered.

3. Next comes the `web server layer`, which could be nxginx as suggested above.

> Sample Varnish config

{% highlight config %}
import directors;
import std;

backend web1 {
  	.host = "t-mw-web-1.internal.targaryen.tech";
  	.port = "80";
  	.connect_timeout = 3s;
  	.first_byte_timeout = 10s;
  	.between_bytes_timeout = 5s;
  	.probe = {
    	.url = "/index.php?title=Main_Page";
    	.expected_response = 200;
    	.timeout = 1s;
    	.interval = 3s;
    	.window = 2;
    	.threshold = 2;
    	.initial = 2;
  	}
}

backend web2 {
  	.host = "t-mw-web-2.internal.targaryen.tech";
  	.port = "80";
  	.connect_timeout = 3s;
  	.first_byte_timeout = 10s;
  	.between_bytes_timeout = 5s;
  	.probe = {
    	.url = "/index.php?title=Main_Page";
    	.expected_response = 200;
    	.timeout = 1s;
    	.interval = 3s;
    	.window = 2;
    	.threshold = 2;
    	.initial = 2;
  	}
}

backend web3 {
  	.host = "t-mw-web-3.internal.targaryen.tech";
  	.port = "80";
  	.connect_timeout = 3s;
  	.first_byte_timeout = 10s;
  	.between_bytes_timeout = 5s;
  	.probe = {
    	.url = "/index.php?title=Main_Page";
    	.expected_response = 200;
    	.timeout = 1s;
    	.interval = 3s;
    	.window = 2;
    	.threshold = 2;
    	.initial = 2;
  	}
}

sub vcl_init {
  	new vdir = directors.round_robin();
  	vdir.add_backend(web1);
  	vdir.add_backend(web2);
  	vdir.add_backend(web3);
}
...
{% endhighlight %}

See a complete [Varnish load balancing setup][varnish-link]{:target="_blank" rel="noopener"} sample.

[nginx-samples]: https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-setup-an-Nginx-load-balancer-example
[nginx-link]: https://upcloud.com/resources/tutorials/configure-load-balancing-nginx
[haproxy-link]: https://www.linode.com/docs/guides/how-to-use-haproxy-for-load-balancing/
[varnish-link]: https://medium.com/@sudhindrasajjal/load-balancing-your-web-application-with-haproxy-varnish-3456a3e5a171
---
layout: post
title:  "Docker build best practices"
date:   2021-11-22 16:53:00 +0200
categories: coding docker image build
excerpt: "I noted down a few useful best practice tips, which might help you better setup and build faster your dev containers, such as lighter base image and decoupled applications."
---

From [official docs][docker-docs]{:target="_blank" rel="noopener"} I noted a few useful best practice tips, which might help you better setup and build faster your dev containers.

<h4>Use multi-stage builds</h4>

Split your Dockerfile instructions into distinct stages to make sure that the resulting output only contains the files that are needed to run the application.

If you have multiple images with a lot in common, consider creating a reusable stage that includes the shared components, and basing your unique stages on that.

<h4>Choose a lighter base image</h4>

When building your own image from a Dockerfile, ensure you choose a minimal base image that matches your requirements. A smaller base image not only offers portability and fast downloads, but also shrinks the size of your image and minimizes the number of vulnerabilities introduced through the dependencies.

You should also consider using two types of base image: one for building and unit testing, and another (typically slimmer) image for production. 

<h4>Exclude with .dockerignore</h4>

To exclude files not relevant to the build, without restructuring your source repository, use a `.dockerignore` file. This file supports exclusion patterns similar to `.gitignore` files.

<h4>Decouple applications</h4>

Each container should have only **one concern**. 

For instance, a **web application stack** might consist of *three separate containers*, each with its own unique image, to manage the web application, database, and an in-memory cache in a decoupled manner.

Use your best judgment to keep containers as clean and modular as possible. If containers depend on each other, you can use **Docker container networks** to ensure that these containers can communicate.

<h4>Bonus - Dockefile best practices: </h4>

**Use current official images as the basis for your images**

Docker recommends the **Alpine image** versions, as it is tightly controlled and small in size (currently under 6 MB), while still being a full Linux distribution.

**Split long or complex RUN statements on multiple lines separated with backslashes**

{% highlight yaml %}
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo
{% endhighlight %}

**Always combine RUN apt-get update with apt-get install in the same RUN statement**

- *Using apt-get update alone in a RUN statement causes caching issues and subsequent apt-get install instructions to fail.*
- *Docker sees the initial and modified instructions as identical and reuses the cache from previous steps.*

{% highlight yaml %}
RUN apt-get update && apt-get install -y \
    aufs-tools \
    automake \
    build-essential \
    curl \
    dpkg-sig \
    libcap-dev \
    libsqlite3-dev \
    mercurial \
    reprepro \
    ruby1.9.1 \
    ruby1.9.1-dev \
    s3cmd=1.1.* \
    && rm -rf /var/lib/apt/lists/*
{% endhighlight %}

**ADD and COPY are functionally similar**

- *COPY supports basic copying of files into the container, from the build context or from a stage in a multi-stage build.*
- *ADD supports features for fetching files from remote HTTPS and Git URLs, and extracting tar files automatically when adding files from the build context.*

**Use USER to change to a non-root user**

If a service can run without privileges, use `USER` to change to a **non-root** user. Start by creating the user and group in the `Dockerfile`.

Read more about [advanced Dockefiles][docker-link1]{:target="_blank" rel="noopener"} and [multi-stage][docker-link2]{:target="_blank" rel="noopener"} build.

[docker-docs]: https://docs.docker.com/build/building/best-practices/
[docker-link1]: https://www.docker.com/blog/advanced-dockerfiles-faster-builds-and-smaller-images-using-buildkit-and-multistage-builds/
[docker-link2]: https://docs.docker.com/build/building/multi-stage/

---
layout: post
title:  "Bitbucket pipelines conditions"
date:   2024-02-14 08:00:00 +0200
categories: coding devops
---

So I started developing a personal project but I was getting too many builds being run, on every commit and every branch.
Why was this an issue? There was a monthly number of pipeline hours included for each account and I was risking to exceed that limit.

I decided to run jobs only when I merge stuff on `main` branch and therefore started investigating how to do this on **Bitbucket**.

I created a `bitbucket-pipelines.yml` file in repo's root and and also secondary files in root folder `build` that contain each jobs' instructions:


{% highlight yaml %}
pipelines:
  branches:
    main:
    - parallel:
      - step:
          name: Build
          script:
            - /bin/bash build/bitbucket-pipelines-build.sh
      - step:
          name: Test
          script:
            - /bin/bash build/bitbucket-pipelines-test.sh
      - step:
          name: Lint
{% endhighlight %}

These options allow you to control the start conditions for your pipelines. Restricting your pipelines to start certain conditions (such as, only when a pull request is created or updated) can reduce the number of build minutes used by your team.

For more information, please check official docs:

[Bitbucket pipelines conditions][bitbucket-pipelines-conditions]{:target="_blank" rel="noopener"}

[bitbucket-pipelines-conditions]: https://support.atlassian.com/bitbucket-cloud/docs/pipeline-start-conditions/

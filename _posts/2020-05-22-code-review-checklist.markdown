---
layout: post
title:  "Code review checklist"
date:   2020-05-22 13:30:00 +0200
categories: coding review pull request
excerpt: "If you work in a team, code review conflicts will surely happen at some point. Try preventing threads noise by following these tips."
---

If you work in a team, opening a **Pull Request** (or **Merge Request**) will surely happen at some point.
Here's an useful [list of things to double check][cr-docs]{:target="_blank" rel="noopener"} before opening it:

<h3>Pick the correct target branch</h3>

Check if your target branch is correct.

<h3>Mark as draft</h3>

Maybe you want some advice and decide to open PRs when the work is mostly done. Make sure to mark as Draft.

<h3>Name things properly</h3>

Start with a name that looks good, but check if the name that you've picked corresponds to the semantic of what you're doing.
`Naming` and `Caching` are the two big things making our life "miserable" :)

<h3>Make sure code quality tools pass</h3>

You should benefit from Linters. 
Linters are tools that analyze your code to identify errors, warnings, style conventions, and complexity issues.
Some things can be fixed on an early stage and not reach the MR at all.

<h3>Remove the noise: stick to ticket</h3>

Avoid making changes that are out of context.
It will only just generate discussions, interpretations and a lot of opened threads on your code.

<h3>Add screenshots and description</h3>

When writing the description, make a small checklist to make explicit how you've advanced in that change.
It provides a clear idea of "next steps" for you and your peers.
I recommend to include one or more screenshots, as sometimes an image speaks 1000 words.

<h3>Be found in the future</h3>

People tend to forget that they can consult the PRs history. You should consider also setting labels to classify them.
If you've never looked for an old PR, think again as it might happen on future projects or features.

<h3>Comment on your changes</h3>

Comment inline any interesting fact or piece of code that you feed the urge to share.
*Make sure to stick to useful comments and keep it short, it must specify something that the code fails to self-explain.*
You can also include ticket ID, doc links or such.

<h4>Bonus: review your own diff</h4>

*As crazy as it sounds, it is that useful. I always create a compare/diff link and review the code myself as if I did not just write it.*

{% highlight config %}

{% endhighlight %}


See a more complete list for [code review checklist][cr-github]{:target="_blank" rel="noopener"} from Github.

[cr-github]: https://gist.github.com/katyhuff/845e06656f18784210190e4f46a4aa95
[cr-docs]: https://hackernoon.com/pull-request-checklist-what-you-need-to-do-before-assigning-a-pr-to-someone-x4263uuk
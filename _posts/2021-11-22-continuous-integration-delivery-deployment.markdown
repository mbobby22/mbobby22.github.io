---
layout: post
title:  "CI/CD integration vs delivery"
date:   2021-11-22 16:55:00 +0200
categories: coding ci cd
---

<h3>Continuous integration</h3>

Developers practicing continuous integration merge their changes back to the main branch as often as possible. The developer's changes are validated by creating a build and running automated tests against the build. By doing so, you avoid integration challenges that can happen when waiting for release day to merge changes into the release branch.
Continuous integration puts a great emphasis on testing automation to check that the application is not broken whenever new commits are integrated into the main branch.

<h3>Continuous delivery</h3>

[Continuous delivery][cdel-link]{:target="_blank" rel="noopener"} is an extension of continuous integration since it automatically deploys all code changes to a testing and/or production environment after the build stage.
This means that on top of automated testing, you have an automated release process and you can deploy your application any time by clicking a button.
In theory, with continuous delivery, you can decide to release daily, weekly, fortnightly, or whatever suits your business requirements. However, if you truly want to get the benefits of continuous delivery, you should deploy to production as early as possible to make sure that you release small batches that are easy to troubleshoot in case of a problem.

<h3>Continuous deployment</h3>
 
[Continuous deployment][cdep-link]{:target="_blank" rel="noopener"} goes one step further than continuous delivery. With this practice, every change that passes all stages of your production pipeline is released to your customers. There's no human intervention, and only a failed test will prevent a new change to be deployed to production.
Continuous deployment is an excellent way to accelerate the feedback loop with your customers and take pressure off the team as there isn't a Release Day anymore. Developers can focus on building software, and they see their work go live minutes after they've finished working on it.

[cdel-link]: https://www.atlassian.com/continuous-delivery
[cdep-link]: https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment
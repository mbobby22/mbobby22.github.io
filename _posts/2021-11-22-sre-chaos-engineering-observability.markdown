---
layout: post
title:  "SRE, Chaos Engineering and Observability"
date:   2021-11-22 16:30:00 +0200
categories: coding sre chaos engineering observability
excerpt: "There is no such thing as 100% reliability, but chaos experiments and the observability in those experiments help you understand why it is so and improve your customer’s confidence in your services."
---

The practice of **Site Reliability**, more specifically, **Chaos Engineering**, has become more mainstream in recent times. From the engineering squads of *Netflix* and *Google* where the practice can trace its foundations, to `SRE engineers` in small retail websites, *‘reliability’* is an important measurement of success.

There is no such thing as 100% reliability but chaos experiments and the observability in those experiments help you understand why it is so and improve your customer’s confidence in your services. Not just react, but observe and react well before your customers do!

<h3>Observability in Chaos Engineering</h3>

- Service Health
- Transactions
- IT Resources
- Relevance
- Telemetry & Instrumentation
- Analytical Insights
- Actions

<h3>Sources of Observable Data</h3>

- Traffic
- Latency
- Errors
- Saturation
- Logging
- Metrics
- Traces

<h2>Observability for a Developer</h2>

If the performance of the service degrades or the service fails, it would inevitably reflect in one or more KPIs.
However, these metrics sometimes show the symptoms of the issue and not necessarily its underlying cause. Hence, at times we may need to look at the observability data from a different perspective.

An `APM` monitoring solution can give us insights about the target micro service at the code level & exposes the vulnerable code segments, if any, from a Developer’s perspective.

There is no place for *‘guess’* or *‘hope’* when it comes to measuring the stability of a service. The data generated out of a chaos experiment now empowers SREs & Service Owners to objectively assess the Reliability of a target service based on real world events.

Read more [here][medium-link]{:target="_blank" rel="noopener"}.

[medium-link]: https://medium.com/@nabtechblog/observability-in-the-realm-of-chaos-engineering-99089226ca51

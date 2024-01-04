---
layout: post
title:  "Lambda for the masses, or why we need a new Go Framework"
date:   2024-01-01 13:44:47 +0100
published: true
categories: gadget cloud aws go
---

# Intro

I built a prototype which demonstrates the feasbility of a go framework to make the usage of lambdas much easier and applicable to a large scale of organizations and use cases.

The prototype is very rough and in a very early alpha stage. The code should show how it feeld to the _users_, i.e. the developers building apps with it and actually deploys and runs to show, that it can be done.

It is more of an invitation to discuss if this is something the world would like to see and how it can become better. The code is already published and there some posts prepared with more details for the next few days. Also I will be working on the Readme.MD files to give some guidance to the people brave enough to give it a go - pun intended.

Lets get right to it....

# Lambda is a niche solution

Using AWS Lambdas requires the least amount of human power and expertise to operate - even at large productive scale deployments. The low operational overhead is offset though by the server usage costs (calculated by allocated memory x CPU size X Compute Duration). Also the amount of scaffolding code (setting up API Gateway routes, getting the deployment buckets ready, linking the resources to be discoverable and the permissions ... oh the pain of policies) can be quite intimidating.

This creates a very small niche in which serverless computing brings all of its benefits:

- the application has a very workload - otherwise compute costs skyrocket
- the application is complex in the amount of consumed sources - otherwise some docker / beanstalk application would be sufficient
- the team is skilled in using various deployment tools - otherwise the team is overburdend with the steep learning-curve.

Wouldn't it be great to broaden this niche?

## The problem of complex set-up

In serverless architecture, every configuration is explicit, leading to a paradox. Setting up an Ubuntu EC2 instance or even a Docker image often seems less cumbersome. Yet, this can result in neglected infrastructure, significant security risks, and escalating complexity over time.

Contrastingly, a basic 'hello-world' lambda linked to an API Gateway demands about 200 lines of Cloudformation YAML. The dilemma is clear: either invest in an infrastructure team for CI/CD stacks maintenance or default to frameworks like Serverless or SST, limiting language choice to Python or NodeJS.

The dream of simply 'throwing' code into the cloud and having it work seamlessly is far from reality with current tools. Here, SST shines, especially with NodeJS, but its magic dims when it comes to other languages. Innovation seems concentrated around JavaScript and Python, given their popularity.


## The problem of prohibitive resource pricing.

Serverless, while convenient, is notably the priciest option for CPU and RAM usage. Lower resource use directly translates to reduced costs, but pre-buying quotas only helps to a certain extent.

Consider the language performance in serverless environments:

![popularity](/images/popularity.png "Language Popularity (by datadog)"){:class="img-responsive"}

Taken from [Datadog](https://www.datadoghq.com/state-of-serverless/) Updated August 2023

The flexibility of the languages allow some _magical_ things to be built into the deployment frameworks, but they come at a high cost: 

![performance](/images/all_languages_performance.png "Performance in Execution Time with Various Memory Profiles"){:class="img-responsive"}

Taken from [Scanner.dev](https://blog.scanner.dev/serverless-speed-rust-vs-go-java-python-in-aws-lambda-functions/) Updated March 2023

While we shouldn't overemphasize a single benchmark, the trend is clear. Go requires about half the memory and is 4 to 6 times faster than Python or Java, offering significant cost savings.

Python or NodeJS suits smaller deployments, where development costs outweigh operational expenses. However, as applications scale, transitioning away from Lambdas or recoding becomes inevitable, again limiting the niche.

Comparing Go and Rust, both offer similar performance, but with different learning curves. Rust, efficient and increasingly popular in systems like the Linux Kernel, can be daunting for average developers. Go, with its simplicity and garbage collector, is a more accessible alternative, especially for Java or Python developers.

### Remote Execution

Remote development and debugging are challenging, particularly for interpreted languages. Compiled languages with strong type-checking, like Go, can mitigate these challenges, streamlining the development process.

The gold standard here is SST's development mode, enabling local runs with native AWS triggers. This feature is yet to be optimized for Go, but can be effectively integrated into a new framework, eliminating frequent deployment interruptions.


## Thesis

Our goal is to harness Go's remarkable efficiency to slash operational costs. We aim to develop a framework that boosts productivity for small teams without dedicated DevOps support. By fully leveraging Go's compiler, we also aim to minimize testing demands, striking a balance between a robust framework and clean, concise business logic.